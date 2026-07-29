---
title: 一次导航到底发生了什么？
url: /inside-browser-part2
---

#### console.info

本文翻译自 `Google` 的官方文档。该系列共 `4` 篇文章，主题为**从内部观察现代浏览器（`Chrome`）**，主要介绍浏览器的内部架构，以及浏览器从输入 `URL` 到页面呈现的全过程。

[原文链接：inside-browser-part2](https://developers.google.com/web/updates/2018/09/inside-browser-part2)

---

## 前言

这是**从内部观察现代浏览器（`Chrome`）**系列文章的第二篇。在上一篇文章中，我们了解了浏览器的组成，以及如何将任务分配给不同的进程和线程。在这篇文章中，我们将深入了解：为了将一个 `Web` 页面呈现在浏览器中，各个进程和线程是如何协作的。

浏览器中都会出现这样一个简单的场景：用户在导航栏里输入 `URL`，接着浏览器从对应的服务器获取数据，并将页面内容呈现出来。在这篇文章中，我们将了解用户请求一个站点后，浏览器准备渲染页面的过程，也就是导航。

## 导航始于 Browser 进程

我们在第一篇文章（[CPU、GPU、内存以及多进程架构][1]）中提到过，除了页面显示区域，其他部分基本都由 `Browser` 进程控制。`Browser` 进程主要有以下几个线程：

线程名              | 作用
---:                | ---
`UI` 线程           | 绘制浏览器头部按钮和导航栏输入框
`network` 线程      | 处理从网络上获取的内容，通过栈的方式进行存储
`storage` 线程      | 处理操作系统的文件权限
...                 | ...

用户能在浏览器的导航栏中输入内容，就是 `UI` 线程在发挥作用。

![图解浏览器主线程内容][2]

## 一次简单的导航

### Step 1：处理输入

当用户在导航栏输入内容时，`UI` 线程要做的第一步，就是判断该内容是一个 `URL` 地址，还是用户想要搜索的内容。因为在 `Chrome` 中，用户可以直接在导航栏输入搜索内容，所以 `UI` 线程需要判断：应该将用户的输入发送给搜索引擎，还是将页面导航到相应站点。

![UI 线程判断用户输入][3]

### Step 2：开始导航

当用户按下 `Enter` 时，`UI` 线程会初始化一个网络连接来获取站点内容，并在标签页左上角显示加载动画。与此同时，网络线程会通过适当的协议（`DNS`），为请求建立 `TLS` 连接。

![UI 线程和网络线程通信将页面导航到 mysite.com][4]

在导航过程中，网络线程有可能会收到服务器返回的重定向响应，比如 `HTTP 301`。在这种情况下，网络线程会通知 `UI` 线程服务器进行了重定向，接着 `UI` 线程会重新初始化一个网络连接，获取重定向站点的内容。

### Step 3：读取响应内容

当网络线程获取到响应内容时，通常会先读取响应头。一般情况下，可以通过响应头中的 `Content-Type` 字段确定响应正文的内容类型。一旦该字段缺失或内容错误，[MIME 类型嗅探][5]就会开始发挥作用。但该策略的实现相当复杂，具体可以查看 [Chrome 中的源码][6]。通过阅读源码，你可以知道不同浏览器是如何确定 `Content-Type` 的。

![响应的结构，Content-Type 以及真实的数据][7]

如果响应数据是一个 `HTML` 文件，下一步就是将该数据交给渲染进程；但如果是一个 `ZIP` 文件（或其他文件），说明这是一次下载请求，数据就会被交给下载管理器处理。

当网络线程获取到响应数据时，它会进行[安全性判断][9]。如果该站点或响应内容匹配到已知的恶意站点，网络线程就会弹出警告页面。此外，网络线程还会进行跨域检查（[Cross Origin Read Blocking（CORB）][10]），确保跨站点的数据不会进入渲染进程。

![网络线程判断该响应是否来自安全站点][8]

### Step 4：查找渲染进程

一旦所有判断都结束，网络线程就可以确认浏览器能够导航到该站点。网络线程会通知 `UI` 线程数据已经准备完毕，之后，`UI` 线程会通知渲染进程渲染页面。

![网络线程通知 UI 线程数据准备完毕，可以进行渲染][11]

当网络线程花费上千毫秒获取响应时，`UI` 线程同时也会进行一些策略上的优化。

由于请求是一个耗时的过程，会造成时间上的浪费。在此期间，`UI` 线程知道浏览器需要导航到哪个页面，因此会在导航时提前准备一个渲染进程。这样，当网络线程获取到内容后，`UI` 线程就可以直接使用已经准备好的渲染进程。但是，如果服务器发生了跨站点重定向，那么准备好的进程可能无法使用，这时就需要使用其他渲染进程。

### Step 5：提交导航

通过前几步，站点数据、渲染进程和 `IPC` 通道都已经准备完毕。`IPC` 会将浏览器进程中的响应数据（`HTML DATA`）以流的形式传递到渲染进程。数据传输完毕后，一次导航就结束了，接下来由渲染进程开始加载文档。

与此同时，地址栏将会更新，安全指示器（`HTTPS`）和地址栏的 `UI` 会反映出站点信息。历史记录会被更新，前进、后退按钮也会指向正确的地址。同时，浏览器会更新存储在本地磁盘中的导航信息，确保选项卡或浏览器关闭后，再次打开时能够获取相应的导航记录。

![IPC 负责将数据从浏览器进程传输到渲染进程][12]

### 额外步骤：加载完成

一旦导航被提交，渲染进程会接收到数据并渲染页面。在下一章中，我们会介绍这一过程。

一旦渲染进程将页面渲染“完毕”（渲染进程解析完数据、显示页面，并执行完所有 `onload` 回调后），它就会通过 `IPC` 通知浏览器进程页面加载完毕。浏览器进程接收到信息后，就会停止选项卡上的 `loading` 状态。

**注：** 这里的“完毕”之所以加引号，是因为客户端的 `JavaScript` 能够继续加载资源，并在资源加载后呈现新的页面内容。

![渲染进程通过 IPC 告知浏览器进程加载完毕][13]

## 导航到其他站点

通过以上步骤，一次简单的导航已经完成。但是，如果用户在地址栏输入了不同的地址，会发生什么呢？浏览器会通过相同的步骤将页面导航至用户输入的站点，但在此之前，它需要查看当前显示的网站是否注册了 `beforeunload` 事件。

当用户想要更换导航地址或关闭选项卡时，站点页面可以通过 `beforeunload` 事件设置“离开当前站点吗？”之类的弹窗，用于提示用户。但是，选项卡内的所有内容，包括 `JavaScript` 代码，都是由渲染进程处理的。因此，当浏览器进程发起新的导航时，需要与当前页面的渲染进程进行交互。

> 注意：不要无条件添加 `beforeunload` 事件。由于这需要进程间通信，过多的 `beforeunload` 事件会造成延迟，因此，该事件应该只在需要时添加。比如，当用户切换导航时，原有页面上用户输入的数据将会丢失。

![浏览器进程通过 IPC 通知渲染进程执行 beforeunload 事件注册的回调][14]

如果导航切换发生在渲染进程内部（比如用户点击 `a` 标签，或触发了 `window.location = "https://newsite.com"`），渲染进程会先触发 `beforeunload` 事件，接着通知浏览器进程切换导航。

当新导航与当前站点不同时，新的导航会生成新的渲染进程，同时旧的渲染进程会在后台处理一些事件，比如 `unload`。你可以阅读 [an overview of page lifecycle states][15]，了解页面的生命周期以及可以使用的事件。

![浏览器进程通过 IPC 通知新老渲染进程执行不同的任务][16]

## Service Worker

最近一个重要的变化，就是浏览器实现了 `Service Worker`。`Service Worker` 可以通过代码实现网络代理；开发者也可以通过 `Service Worker` 缓存从网络上获取的数据。如果网站使用了 `Service Worker`，那么该页面就可以从缓存中获取数据，而不需要从网络上拉取。

但需要注意的是，`Service Worker` 是用 `JavaScript` 编写的，而 `JavaScript` 在渲染进程中执行，导航却发生在浏览器进程中。那么，浏览器进程是如何知道该站点注册了 `Service Worker` 的呢？

当 `Service Worker` 注册成功后，它的作用域将会被保存下来（具体可查看 [Service Worker 的生命周期][17]）。当导航发生时，网络线程会确认该站点是否注册了 `Service Worker`。如果已经注册，那么 `UI` 线程会创建渲染进程，执行 `Service Worker` 的相关代码。该 `Service Worker` 会从缓存中获取数据，而不是通过网络获取。当然，如果资源未缓存，仍会从网络上拉取。

![浏览器进程中的网络线程查看该站点是否注册了 Service Worker][18]
![浏览器进程中的 UI 线程创建渲染进程并执行 Service Worker][19]

## 导航预加载

当页面注册了 `Service Worker`，但相关资源没有被缓存时，渲染进程就需要通知浏览器进程获取数据，而进程间通信会造成一定程度的延迟。为了减轻延迟的影响，[导航预加载][20]是一种有效的策略。该策略会在 `Service Worker` 执行的同时从网络加载数据。它通过设定特定的请求头，允许服务器决定返回的数据。比如，浏览器可能只需要 `HTML` 文档的一部分，而不是整个文档。

![浏览器进程中的 UI 线程在通知渲染进程处理 Service Worker 的同时通知网络线程获取数据][21]

## 总结

在这篇文章中，我们了解了一次导航中发生了什么，以及站点中的 `JavaScript` 代码是如何影响浏览器各个进程的。知道浏览器从网络上获取数据的具体步骤后，我们就能更轻松地理解浏览器为什么要进行导航预加载。在下一篇文章中，我们将深入渲染进程，了解浏览器是如何通过 `HTML/CSS/JavaScript` 代码渲染出一个页面的。

## 相关文章

- [CPU、GPU、内存以及多进程架构](https://breeze.vin/inside-browser-part1/)
- [一次导航到底发生了什么？](https://breeze.vin/inside-browser-part2/)
- [详解渲染进程](https://breeze.vin/inside-browser-part3/)
- [事件合成器](https://breeze.vin/inside-browser-part4/)

[1]: /inside-browser-part1
[2]: https://pic.breeze.red/post/t-chrome-blog-browserprocesses.png
[3]: https://pic.breeze.red/post/t-chrome-blog-input.png
[4]: https://pic.breeze.red/post/t-chrome-blog-navstart.png
[5]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types
[6]: https://cs.chromium.org/chromium/src/net/base/mime_sniffer.cc?sq=package:chromium&dr=CS&l=5
[7]: https://pic.breeze.red/post/t-chrome-blog-response.png
[8]: https://pic.breeze.red/post/t-chrome-blog-sniff.png
[9]: https://safebrowsing.google.com/
[10]: https://www.chromium.org/Home/chromium-security/corb-for-developers
[11]: https://pic.breeze.red/post/t-chrome-blog-findrenderer.png
[12]: https://pic.breeze.red/post/t-chrome-blog-commit.png
[13]: https://pic.breeze.red/post/t-chrome-blog-loaded.png
[14]: https://pic.breeze.red/post/t-chrome-blog-beforeunload.png
[15]: https://developers.google.com/web/updates/2018/07/page-lifecycle-api
[16]: https://pic.breeze.red/post/t-chrome-blog-unload.png
[17]: https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle
[18]: https://pic.breeze.red/post/t-chrome-blog-scope-lookup.png
[19]: https://pic.breeze.red/post/t-chrome-blog-serviceworker.png
[20]: https://developers.google.com/web/updates/2017/02/navigation-preload
[21]: https://pic.breeze.red/post/t-chrome-blog-navpreload.png
