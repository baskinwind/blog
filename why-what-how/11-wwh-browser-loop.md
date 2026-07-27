---
title: 浏览器事件循环
url: /wwh-browser-loop
---
## 前言

`javascript` 是以单线程的形式运行在宿主环境下，`javascript` 采用了回调的形式来解决异步任务。

### 为什么是单线程？

`javascript` 的最开始的出现是为了给 `web` 页面增添一些动态的效果，那么就避免不了获取页面上的元素信息，如果 `javascript` 是以多线程的形式运行在浏览器内，如果两个线程内的 `javascript` 同时去获取/修改，某个页面上的元素，那么浏览器该让哪个 `javascript` 线程拥有获取/修改该元素的权限呢？由于元素的信息会经常性的发生变化，那么又改如何去同步各个线程内所保存的元素信息呢？

所以综合以上问题， `javascript` 是单线程的原因就显示意见了，单线程在执行时，对于元素信息的引用在同一时间仅可能只有一个，那么以上所有的问题都不存在了。

### 什么是异步任务？

任何代码在执行时，都会碰到一些需要经过大量时间运算或是等待的代码，在浏览器的环境下，常见的就是 `http` 任务，比如：资源的加载（图片的 `onload` 事件），`ajax` 的请求（`XMLHttpRequest` 的 `onLoad` 事件）还有页面元素的点击事件以及定时器等。

以上的任务都极其的耗时而且会受环境的影响，如果同步执行的话就会造成 `javascript` 执行的卡顿，而 `javascript` 又是单线程的形式存在在浏览器端，为了使得 `javascript` 的执行不受到影响，`javascript` 会将这些任务执行放在另一个环境下，而将这些任务执行完成后的需要执行的函数给保存下来（也就是回调），这也是为什么一定要写一个回调的原因。当另一个环境下通知 `javascript` 线程该任务已完成，并将任务数据给到 `javascript` 线程，`javascript` 再去保存的回调中寻找该任务对应的回调，将数据当做参数并执行该回调。

## Event Loop

上面大概简述了下 `javascript` 为什么要以异步回调的形式来处理一些耗时任务，那么接下来就说说 `javascript` 到底是如何处理这些异步回调的。

从代码入手

```
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

`javascript` 会从上到下执行该代码，当执行到 `image.src = 'image url'` 时，`javascript` 线程通知浏览器图片加载程序去加载相应图片，然后 `javascript` 继续执行剩下的代码，当执行到 `onload` 和 `onerror` 时，`javascript` 仅仅是保存了这两个函数而已（保存回调）。

当浏览器图片加载程序加载好图片，就会通知 `javascript` 线程， `image` 已加载完毕，如果没有发生错误，那么 `javascript` 在接收到该信号以后就会执行 `image.onload` 方法，如果通知回来是加载失败，那么就会执行 `image.onerror` 方法。

### 事件队列

按照上面所说，并结合最开始说的，如果图片加载程序加载好图片返回加载成功的信号时 `javascript` 正在处理别的任务，由于 `javascript` 是单线程不能同时处理多个任务，那么这个加载成功的信号就会被搁置，放在一个事件队列中，`javascript` 线程在处理好当前的任务后就会去事件队列中取出一个事件并执行响应的回调。

### Loop

在真正的浏览器环境下，异步任务的信号每时每刻都会发生（比如设置的定期器，用户的行为，`ajax`等），那么每时每刻都会有新的任务信号进入事件队列中，所以在浏览器中 `javascript` 的执行会有以下的效果：

以下为 `javascript` 线程执行的内容

1. 加载 `script` 所对应的 `javascript` 脚本
2. 执行 `javascript` 代码，注册异步任务，保存回调函数
3. 引入的脚本所有代码执行完毕
4. 一些 `UI` 渲染（该步骤不一定会有）
5. 取事件队列中最早进入的事件，并在事件队列中删除该事件
6. 执行该事件对应的回调代码
7. 回调代码执行完毕
8. 一些 `UI` 渲染（该步骤不一定会有）
9. 回到步骤 `5`

- `1 - 4` 步是浏览器加载 `javascript` 所必须执行的，可以认为是注册异步任务最开始的地方。
- 步骤 `6` 执行回调的过程中可能会产生新的回调，比如在 `ajax` 请求成功回调中注册了页面元素的点击事件

以下为浏览器相关程序的内容（异步任务）

1. 接收到 `javascript` 注册的异步任务
2. 执行任务
3. 任务完成后在事件队列中推入成功事件
4. 任务失败后在事件队列中推入失败事件

这样下来，`javascript` 线程就会持续不断的执行，也不会因为耗时任务而暂停执行。

`javascript` 线程中 `5 - 9` 步就是在浏览器下的 `Event Loop` 。

### 图解

![浏览器下的 Event Loop](https://pic.breeze.red/wwh-wbe-eventloop.png)

- heap              回调函数保存处（堆）
- stack             可以认为是主线程执行的地方（栈）
- callback queue    事件队列
- WebAPIs           浏览器中处理 `javascript` 发出异步任务的程序

## macro task 与 micro task

`ES6` 出现之前，只有一个事件队列，`ES6` 出现后，多了一个事件队列，叫 `micro task` （微任务），用来专门放在一些优先级较高的任务，而之前实现的事件队列就叫做 `macro task` （宏任务）。

那么多了一个事件队列，事件的读取也发生了变化

1. 加载 `script` 所对应的 `javascript` 脚本
2. 执行 `javascript` 代码，注册异步任务，保存回调函数
3. 引入的脚本所有代码执行完毕
4. 一些 `UI` 渲染（该步骤不一定会有）
5. 读取微任务事件队列中最早进入的事件并删除该事件，有则进入下一步，没有执行第 `7` 步
6. 执行该任务对应的回调，执行结束后回到第 `5` 步
7. 读取宏任务事件队列中最早进入的事件
8. 执行该任务事件对应的回调
9. 读取微任务事件队列中最早进入的事件并删除该事件，有则进入下一步，没有执行第 `11` 步
10. 执行该任务对应的回调，执行结束后回到第 `9` 步
11. 宏任务回调代码执行完毕
12. 一些 `UI` 渲染（该步骤不一定会有）
13. 回到步骤 `5`

就是当每次 `javascript` 线程任务执行结束后，会优先处理微任务事件队列中的事件，与宏任务不一样的地方在于，浏览器会将微任务事件队列中的事件一次性全部处理完在进行 `UI` 渲染。

### 图解

![微任务与宏任务](https://pic.breeze.red/wwh-wbe-ma%28i%29crotask.png)

能产生微任务的方式：

- MutationObserver
- Promise.then catch finally

能产生宏任务的方式：

- setTimeout
- setInterval
- 用户行为
- Image#onload
- XMLHttpRequest
- requestAnimationFrame

## 参考

- [event-loop][1]
- [JavaScript 运行机制详解：再谈Event Loop][2]
- [深入理解js事件循环机制（浏览器篇）][3]

[1]: https://www.w3.org/TR/html5/webappapis.html#event-loops
[2]: http://www.ruanyifeng.com/blog/2014/10/event-loop.html
[3]: http://lynnelv.github.io/js-event-loop-browser
