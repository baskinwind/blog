---
title: 浏览器事件循环
url: /wwh-browser-loop
---

## 前言

`JavaScript` 以单线程的形式运行在宿主环境下，并采用回调的形式来解决异步任务。

### 为什么是单线程？

`JavaScript` 最开始出现，是为了给 `Web` 页面增添一些动态效果，那么就避免不了获取页面上的元素信息。如果 `JavaScript` 以多线程的形式运行在浏览器内，两个线程内的 `JavaScript` 同时获取或修改某个页面上的元素，那么浏览器该让哪个 `JavaScript` 线程拥有获取或修改该元素的权限呢？由于元素信息会经常发生变化，那么，又该如何同步各个线程内所保存的元素信息呢？

所以，综合以上问题，`JavaScript` 是单线程的原因就显而易见了。单线程执行时，对于元素信息的引用在同一时间仅可能只有一个，那么以上所有问题都不存在了。

### 什么是异步任务？

任何代码在执行时，都会碰到一些需要大量时间运算或等待的代码。在浏览器环境下，常见的就是 `HTTP` 任务，比如资源加载（图片的 `onload` 事件）、`Ajax` 请求（`XMLHttpRequest` 的 `onload` 事件），还有页面元素的点击事件以及定时器等。

以上任务都极其耗时，而且会受环境影响。如果同步执行，就会造成 `JavaScript` 执行卡顿；而 `JavaScript` 又以单线程的形式存在于浏览器端。为了使 `JavaScript` 的执行不受影响，`JavaScript` 会将这些任务放在另一个环境中执行，并保存任务完成后需要执行的函数（也就是回调），这也是为什么一定要写回调的原因。当另一个环境通知 `JavaScript` 线程该任务已经完成，并将任务数据交给 `JavaScript` 线程后，`JavaScript` 再从已保存的回调中寻找该任务对应的回调，将数据作为参数并执行该回调。

## Event Loop

上面大致说明了 `JavaScript` 为什么要以异步回调的形式处理一些耗时任务，那么接下来就说说 `JavaScript` 到底是如何处理这些异步回调的。

从代码入手：

```javascript
// a.js
let image = new Image();
image.src = 'image url';

image.onload = () => {
    // image 加载成功回调
}
image.onerror = () => {
    // image 加载失败回调
}
```

`JavaScript` 会从上到下执行该代码。当执行到 `image.src = 'image url'` 时，`JavaScript` 线程通知浏览器图片加载程序去加载相应图片，然后继续执行剩下的代码。当执行到 `onload` 和 `onerror` 时，`JavaScript` 仅仅是保存了这两个函数（保存回调）。

当浏览器图片加载程序加载好图片后，就会通知 `JavaScript` 线程：`image` 已加载完毕。如果没有发生错误，那么 `JavaScript` 在接收到该信号后，就会执行 `image.onload` 方法；如果收到的是加载失败通知，那么就会执行 `image.onerror` 方法。

### 事件队列

按照上面所说，并结合最开始提到的内容，如果图片加载程序返回加载成功信号时，`JavaScript` 正在处理其他任务，那么由于 `JavaScript` 是单线程，不能同时处理多个任务，这个加载成功信号就会被搁置，放入事件队列中。`JavaScript` 线程处理完当前任务后，就会从事件队列中取出一个事件，并执行相应的回调。

### Loop

在真正的浏览器环境下，异步任务的信号每时每刻都会发生（比如设置的定时器、用户行为、`Ajax` 等），每时每刻都会有新的任务信号进入事件队列中。所以，在浏览器中，`JavaScript` 的执行会有以下效果：

以下为 `JavaScript` 线程执行的内容：

1. 加载 `script` 所对应的 `JavaScript` 脚本。
2. 执行 `JavaScript` 代码，注册异步任务，保存回调函数。
3. 引入脚本的所有代码执行完毕。
4. 进行一些 `UI` 渲染（该步骤不一定会有）。
5. 取出事件队列中最早进入的事件，并从事件队列中删除该事件。
6. 执行该事件对应的回调代码。
7. 回调代码执行完毕。
8. 进行一些 `UI` 渲染（该步骤不一定会有）。
9. 回到步骤 `5`。

- `1 - 4` 步是浏览器加载 `JavaScript` 时必须执行的，可以认为是最开始注册异步任务的地方。
- 步骤 `6` 执行回调的过程中可能会产生新的回调，比如在 `Ajax` 请求成功的回调中注册页面元素的点击事件。

以下为浏览器相关程序执行的内容（异步任务）：

1. 接收到 `JavaScript` 注册的异步任务。
2. 执行任务。
3. 任务完成后，在事件队列中推入成功事件。
4. 任务失败后，在事件队列中推入失败事件。

这样一来，`JavaScript` 线程就会持续不断地执行，也不会因为耗时任务而暂停。

`JavaScript` 线程中的 `5 - 9` 步，就是浏览器下的 `Event Loop`。

### 图解

![浏览器下的 Event Loop][4]

- `heap`：回调函数保存处（堆）。
- `stack`：可以认为是主线程执行的地方（栈）。
- `callback queue`：事件队列。
- `WebAPIs`：浏览器中处理 `JavaScript` 发出的异步任务的程序。

## macro task 与 micro task

`ES6` 出现之前，只有一个事件队列。`ES6` 出现后，多了一个叫作 `micro task`（微任务）的事件队列，专门用来存放一些优先级较高的任务，而之前实现的事件队列就叫作 `macro task`（宏任务）。

那么，多了一个事件队列后，事件的读取也发生了变化：

1. 加载 `script` 所对应的 `JavaScript` 脚本。
2. 执行 `JavaScript` 代码，注册异步任务，保存回调函数。
3. 引入脚本的所有代码执行完毕。
4. 进行一些 `UI` 渲染（该步骤不一定会有）。
5. 读取微任务事件队列中最早进入的事件并删除该事件；如果有，则进入下一步，如果没有，则执行第 `7` 步。
6. 执行该任务对应的回调，执行结束后回到第 `5` 步。
7. 读取宏任务事件队列中最早进入的事件。
8. 执行该任务事件对应的回调。
9. 读取微任务事件队列中最早进入的事件并删除该事件；如果有，则进入下一步，如果没有，则执行第 `11` 步。
10. 执行该任务对应的回调，执行结束后回到第 `9` 步。
11. 宏任务回调代码执行完毕。
12. 进行一些 `UI` 渲染（该步骤不一定会有）。
13. 回到步骤 `5`。

也就是说，每次 `JavaScript` 线程任务执行结束后，都会优先处理微任务事件队列中的事件。与宏任务不同的是，浏览器会将微任务事件队列中的事件一次性全部处理完，再进行 `UI` 渲染。

### 图解

![微任务与宏任务][5]

能产生微任务的方式：

- `MutationObserver`
- `Promise.then`、`catch`、`finally`

能产生宏任务的方式：

- `setTimeout`
- `setInterval`
- 用户行为
- `Image#onload`
- `XMLHttpRequest`
- `requestAnimationFrame`

## 参考

- [event-loop][1]
- [JavaScript 运行机制详解：再谈 Event Loop][2]
- [深入理解 JS 事件循环机制（浏览器篇）][3]

[1]: https://www.w3.org/TR/html5/webappapis.html#event-loops
[2]: https://www.ruanyifeng.com/blog/2014/10/event-loop.html
[3]: https://lynnelv.github.io/js-event-loop-browser

[4]: https://pic.breeze.red/post/wwh-wbe-eventloop.png
[5]: https://pic.breeze.red/post/wwh-wbe-ma%28i%29crotask.png
