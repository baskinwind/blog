---
title: Node.js 事件循环
url: /wwh-node-loop
---
## 前言

通过 [浏览器下的 Event Loop][1] ，可以得知 `javascript` 是一门事件驱动的语言， `javascript` 主线程通过不断的调用事件队列中的事件来完成异步任务。那么 `Node` 下是否也是如此？

## `Node` 下的事件队列

在 `Node` 官网有这样一篇文章：[The Node.js Event Loop, Timers, and process.nextTick()][2] 。
该文章主要讲述了 `Node` 中是如何处理以及实现 `Node` 下的 `Event Loop`。

主要包含以下内容：

### 事件队列

不同于浏览器，`Node` 下的有 6 个事件处理阶段，其执行顺序和队列名称如下：

```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```

不同事件队列的含义如下：

阶段名                  | 含义
---                     | ---
`timers`                | 处理由 `setTimeout`、`setInterval` 产生的回调
`pending callbacks`     | 处理由系统调用错误产出的回调，比如网络连接失败的情况
`idle, prepare`         | `Node` 内部使用
`poll`                  | 处理由 `I/O` 产生的事件
`check`                 | 处理由 `setImmediate` 产生的回调
`close callbacks`       | 处理由某些对象发出的 `close` 事件，比如 `socket.on('close', ...)`

### timers 阶段

`timers` 阶段是 `Node` 下 `Event Loop` 的第一个阶段，在该阶段下，`Node` 会检查所有注册的定时器是否到期，如果到期则压入 `timers` 的任务队列中，检查完毕后，依次执行任务队列中的任务。

**注：** 在 `Node` 下设置定义器，不能保证在设置的时间到达后立即执行，`Node` 只能保证在设置的时间到达后，尽可能快的执行设置的回调。

### pending callbacks 阶段

该阶段的任务队列主要由操作系统发出的任务失败的事件构成，比如网络连接失败。

### poll 阶段

该阶段为 `Node` 中最重要的一个阶段，`Node` 下绝大多数的异步任务都会在这个阶段进行处理，而不像其他阶段都是同种类型的任务。

在进入该阶段前，会进行一次定时器的统计，找出最近一次需要触发的定时器，避免 `Node` 被阻塞在改阶段而定时器得不到运行。

进入该阶段后，会发生以下事情

1. 计算出有多久的时间可以用来做任务的触发
2. 依次执行该事件队列中的事件

`Event Loop` 进入了 `poll` 阶段，根据定时器的设置情况，会发生以下事情

#### 未设置定时器

1. 如果该阶段的事件队列不为空，`Node` 将同步执行事件队列中的事件回调，直至该队列为空，或执行的回调达到系统上限。
2. 如果该阶段的事件队列为空，那么系统将根据有没设置 `setImmediate` 做出相应的处理
    - 设置了 `setImmediate` 那么该阶段结束，进入 `check` 阶段
    - 未设置 `setImmediate` 那么 `Node` 将阻塞在改阶段，继续接收产生的事件并处理

由于在 `Node` 下该阶段接收了绝大多数的事件，而且该阶段内的事件都不是固定产生的，对比定时器的固定时间触发，关闭事件由 `close` 事件触发（ `close` 事件由程序主动触发），因此该阶段应该是 `Event Loop` 中最重要的一个阶段，所以程序应该尽可能的留在该阶段处理事件，当没有定时器，没有设置 `setImmediate` 那么 `Node` 进行 `Event Loop` 也没有意义， `Node` 只需要停留在该阶段，继续处理事件即可。

#### 设置了定时器

当该阶段的任务队列处理结束，`Node` 会检查是否有定时器到期，如果有一个以上定时器已到期，`Event Loop` 会结束该阶段，进入一下个阶段。

### check 阶段

该阶段的任务队列中保存着在该阶段执行前，所有由 `setImmediate` 注册的回调。

### close callbacks 阶段

该阶段的任务队列主要由 `close` 发出的事件构成，比如 `socket.on('close', ...)` 。

## micro task

同样的在 `Node` 下的 `Event Loop` 也有一个为任务队列，在 `Node` 下产生微任务的方式

- `process.nextTick()`
- `Promise.then` `catch` `finally`

不同于浏览器端，微任务队列会在 `Event Loop` 每个阶段执行结束后执行，每次执行会将微任务队列中的所有任务执行完毕。

### 图解

![node 微任务 & 宏任务](https://pic.breeze.red/wwh-wne-ma%28i%29crotask-in-node.png)

## 对比浏览器

```js
setTimeout(()=>{
    console.log('timer1')

    Promise.resolve().then(function() {
        console.log('promise1')
    })
}, 0)

setTimeout(()=>{
    console.log('timer2')

    Promise.resolve().then(function() {
        console.log('promise2')
    })
}, 0)
```

以上代码在浏览器和 `Node` 下会得到不同的结果，图解如下

浏览器

```
timer1
promise1
timer2
promise2
```

`Node`

```
timer1
timer2
promise1
promise2
```

浏览器下任务执行过程

![浏览器任务执行顺序](https://pic.breeze.red/wwh-wne-browser-excute-animate.gif)

`Node` 下任务执行过程

![Node 任务执行顺序](https://pic.breeze.red/wwh-wne-node-excute-animate.gif)

## 总结

1. 在 `Node` 下有 `7` 个任务队列，分别对应 `6` 个 `Event Loop` 阶段，和一个微任务队列
2. `poll` 阶段处理了觉大多数的异步任务，如果其他阶段没任务，`Event Loop` 会在这里停留等待新的任务
3. `Node` 下为任务的执行时机在每个阶段结束后，而浏览器在每个宏任务结束后

最后更具前面的内容，得出的在 `Node` 下 `Event Loop` 下的执行过程如下：

![Node 下 Event Loop](https://pic.breeze.red/wwh-wne-node-event-loop.png)

- 橙色为 `Event Loop` 的主要内容
- 蓝色为同一个为任务队列
- 褐色为 `poll` 阶段内部的一个循环
- 黑色为进出 `poll` 阶段的逻辑判断

## 参考

- [The Node.js Event Loop, Timers, and process.nextTick()][2]
- [深入理解js事件循环机制（Node.js篇）][3]

[1]: https://breeze.red/browser-event-loop/
[2]: https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/
[3]: http://lynnelv.github.io/js-event-loop-nodejs
