---
title: 事件合成器
url: /inside-browser-part4
---

#### console.info

本文翻译自 `Google` 的官方文档。该系列共 `4` 篇文章，主题为**从内部观察现代浏览器（`Chrome`）**，主要介绍浏览器的内部架构，以及浏览器从输入 `URL` 到页面呈现的全过程。

[原文链接：inside-browser-part4](https://developers.google.com/web/updates/2018/09/inside-browser-part4)

---

## 前言

这是**从内部观察现代浏览器（`Chrome`）**系列文章的第四篇。在上一篇文章中，我们知道了浏览器是如何将代码转化成网页的。在这篇文章中，我们将了解事件合成器是如何顺畅地处理用户交互的。

## 从浏览器的角度看待用户的输入

何为输入？使用键盘在文本框中输入，或是使用鼠标点击？是的，这些都是。但从浏览器的角度来看，输入意味着用户与网页交互的所有行为，比如鼠标点击、鼠标移动、滚轮滚动、触摸事件等等。

那么，浏览器如何处理输入呢？当用户触发 `touch` 事件时，浏览器进程最先知道事件已经发生，但它只知道事件发生的坐标。因为选项卡中的内容完全由渲染进程控制，所以浏览器进程并不知道具体触发了哪个元素。因此，浏览器进程只能通过 `IPC` 将事件类型和发生坐标发送给渲染进程，由渲染进程进一步处理。渲染进程会根据事件类型和坐标寻找元素，并触发与该元素绑定的事件回调。

![输入事件通过浏览器进程到渲染进程][1]

## 合成器处理输入事件

在上一篇文章中，我们知道合成器会分别对各个分层进行光栅化，使页面能够平滑、流畅地滚动。如果页面不需要处理用户输入，那么合成线程可以独立生成新的合成帧，而不需要经过主线程（不需要执行 `JavaScript` 代码）。但如果页面上的元素绑定了处理用户输入的相关事件，合成线程该如何处理呢？因为它不具备执行 `JavaScript` 代码的能力，只能交给渲染进程中的主线程处理。

## 非快速可滚动区域

当 `JavaScript` 代码在主线程中执行并生成分层，合成线程将这些分层进行合成时，会判断并标记分层中需要处理用户输入的区域（绑定了事件的元素），这些区域被称为“非快速可滚动区域”。合成线程处理用户输入时，如果输入发生在这些标记区域，就会通知主线程处理；如果发生在未标记区域，那么合成线程只需要重新生成合成帧即可。

![红色区域即为非快速可滚动区域][2]

## 绑定事件时需要知道的注意点

浏览器通过事件委托实现事件绑定。由于存在事件冒泡机制，开发者可以在页面的根元素上绑定事件，统一处理所有事件，如下面的代码所示：

```js
document.body.addEventListener('touchstart', event => {
    if (event.target === area) {
        event.preventDefault();
    }
});
```

通过上面的代码，页面上所有元素的 `touchstart` 事件都会在这里处理。但是，在浏览器看来，页面的根元素（`body`）绑定了事件，那么它所对应的分层及其包含的分层，就都是非快速可滚动区域。这意味着，即使页面不需要处理用户输入，合成线程也要在用户输入时通知主线程处理事件，即使绑定的处理函数不会改变页面布局。这个过程不可避免地需要耗费时间，因此，合成线程分层处理的意义也会被削弱。

![非快速可滚动区域覆盖了整个页面][3]

当然，也不是没有解决办法。开发者可以通过 `passive: true` 选项告知浏览器：事件需要监听，但合成线程不需要等待主线程执行完毕，可以直接继续生成新的合成帧。

```js
document.body.addEventListener('touchstart', event => {
    if (event.target === area) {
        event.preventDefault();
    }
}, { passive: true });
```

## 事件的取消

假设手机页面上有一个元素，你想限制它的滚动方向，只允许用手指水平拖动，而不允许垂直拖动。

![元素只能水平拖动][4]

通过上面的介绍，使用 `passive: true` 选项可以让元素流畅地滚动。但是，当主线程执行回调时，元素其实已经发生了滚动（合成线程和主线程同时执行），这会造成行为上的偏差。并且，由于滚动已经发生，再使用 `preventDefault` 也来不及了。因此，如果使用了 `passive: true`，那么事件回调中就不能使用 `preventDefault`。以下代码会在浏览器中触发警告：

```js
document.body.addEventListener('pointermove', event => {
    event.preventDefault(); // Unable to preventDefault inside passive event listener invocation.
}, { passive: true });
```

> 原文有误，因为 `cancelable` 属性并没有变化。这一结论来自 [MDN 文档中关于 passive 属性的介绍][5]。

## 确定触发节点

在合成线程将事件通知主线程前，还需要确定触发节点。合成线程会结合事件触发坐标和主线程生成的绘制记录，确定用户触发事件时对应的 `DOM` 元素。

![合成线程寻找需要触发事件的元素][6]

## 降低触发频率

在上一篇文章中，我们讨论过，屏幕的刷新频率通常为每秒 `60` 帧。为了确保页面效果流畅，代码也应该按照这个频率执行。但是，在触摸屏上，`touch` 事件每秒可以触发 `60-120` 次，鼠标事件也可以达到每秒 `100` 次。因此，用户的输入频率远高于屏幕的刷新频率。

以 `touchmove` 事件为例，该事件每秒触发 `120` 次，会造成合成线程多次寻找触发节点并通知主线程。但其中绝大多数事件其实都不需要单独处理，因为屏幕的刷新频率显然跟不上。

![过多的事件导致屏幕刷新跟不上][7]

为了降低事件的触发频率，`Chrome` 会将连续事件合并（比如 `wheel`、`mousewheel`、`mousemove`、`pointermove`、`touchmove`），并延迟触发，直到上一帧中的内容处理完毕。这和使用 `requestAnimationFrame` 防止页面卡顿是同一个原理。

![合并和被延迟的事件][8]

但是，独立触发的事件（比如 `keydown`、`keyup`、`mouseup`、`mousedown`、`touchstart`、`touchend`）会直接触发。

## 获取合并事件

绝大多数情况下，合并事件能够给用户带来良好的体验。但是，如果网页通过事件进行绘画（根据 `touchmove`），那么事件合并就会导致画出的线条不够流畅，因为许多轨迹被合并掉了。在这种情况下，可以通过 `getCoalescedEvents` 方法获取那些被合并事件的内容。

![平滑的滑动轨迹因为事件合成被处理成了直线][9]

实现右边绘画的大致代码如下：

```js
window.addEventListener('pointermove', event => {
    const events = event.getCoalescedEvents();
    for (let event of events) {
        const x = event.pageX;
        const y = event.pageY;
        // draw a line using x and y coordinates.
    }
});
```

## 接下来？

在这个系列中，我们深入了解了浏览器是如何将网页呈现在屏幕上的，也了解了为什么 `DevTools` 建议开发者在事件上添加 `passive: true`，以及为什么需要在 `script` 上添加 `async` 属性。通过这个系列的文章，大家也应该明白，浏览器需要这些信息，才能提供更快、更流畅的 `Web` 体验。

### 使用 Lighthouse

如果你想优化代码，却不知道从何做起，使用 [`Lighthouse`][10] 是一个不错的选择。它可以检查网页，并生成一个需要改进的列表。通过该列表，你就可以知道浏览器需要你做什么，才能给用户带来流畅的体验。

### 衡量网页性能

不同网站对性能的要求各不相同，因此，衡量网站性能并制定相应策略至关重要。至于如何制定，可以查看 `Chrome DevTools` 团队的文章：[how to measure your site's performance][11]。

### 在站点中添加 Feature Policy

[`Feature Policy`][12] 是 `Web` 平台的一项新功能，可以确保应用顺利构建。启用功能策略，可以确保应用程序稳定运行。比如，如果你想让 `JavaScript` 代码不中断文档解析，开启 `synchronous scripts policy`（同步脚本策略）即可。当启用 `sync-script: 'none'` 时，解析器将会阻止 `JavaScript` 执行。这样，你就不用考虑 `JavaScript` 代码是否会修改文档，浏览器也不用担心 `JavaScript` 的执行导致 `DOM` 发生变化。

## 总结

当我们开始构建网站时，往往只关心如何编写代码，以及如何寻找生产力工具来提高效率。但是，思考浏览器如何处理我们编写的代码也非常重要。现代浏览器持续投入大量资源，为用户提供良好的 `Web` 体验；而编写良好的代码同样能够提升用户体验，这是我们共同的目标。

## 相关文章

- [CPU、GPU、内存以及多进程架构](https://breeze.vin/inside-browser-part1/)
- [一次导航到底发生了什么？](https://breeze.vin/inside-browser-part2/)
- [详解渲染进程](https://breeze.vin/inside-browser-part3/)
- [事件合成器](https://breeze.vin/inside-browser-part4/)

[1]: https://pic.breeze.red/post/t-chrome-blog-input4.png
[2]: https://pic.breeze.red/post/t-chrome-blog-nfsr1.png
[3]: https://pic.breeze.red/post/t-chrome-blog-nfsr2.png
[4]: https://pic.breeze.red/post/t-chrome-blog-scroll.png
[5]: https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener
[6]: https://pic.breeze.red/post/t-chrome-blog-hittest.png
[7]: https://pic.breeze.red/post/t-chrome-blog-rawevents.png
[8]: https://pic.breeze.red/post/t-chrome-blog-coalescedevents.png
[9]: https://pic.breeze.red/post/t-chrome-blog-getCoalescedEvents.png
[10]: https://developers.google.com/web/tools/lighthouse/
[11]: https://developers.google.com/web/tools/chrome-devtools/speed/get-started
[12]: https://developers.google.com/web/updates/2018/06/feature-policy
