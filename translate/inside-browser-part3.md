---
title: 详解渲染进程
url: /inside-browser-part3
---

#### console.info

本文翻译自 `Google` 的官方文档。该系列共 `4` 篇文章，主题为**从内部观察现代浏览器（`Chrome`）**，主要介绍浏览器的内部架构，以及浏览器从输入 `URL` 到页面呈现的全过程。

[原文链接：inside-browser-part3](https://developers.google.com/web/updates/2018/09/inside-browser-part3)

---

## 前言

这是**从内部观察现代浏览器（`Chrome`）**系列文章的第三篇。在先前的文章中，我们了解了浏览器的多进程架构，以及一次导航中到底发生了什么。在这篇文章中，我们将了解渲染进程到底做了什么。

渲染进程直接关系到 `Web` 页面的性能。由于渲染进程涉及许多内容、处理大量逻辑，并不是一篇文章能够完整描述的，因此，这篇文章只是一个概述。如果你想进一步了解，可以查看 [the Performance section of Web Fundamentals][1]。

## 主要构成

选项卡内部的所有逻辑都由渲染进程处理。在渲染进程中，主线程处理绝大部分网页代码。由 `Worker` 或 `Service Worker` 注册的 `JavaScript` 代码，会交给单独的 `Worker` 线程处理。`Compositor`（合成器）和 `Raster`（光栅）线程，则确保页面快速、平滑地呈现。

渲染进程最重要的任务，就是将 `HTML`、`CSS`、`JavaScript` 代码转换成可以与用户交互的界面。

![渲染进程拥有的几个主要的线程][2]

## 解析

### 构建 DOM

当渲染进程接收到导航的确认信息，并开始接收响应数据（`HTML data`）时，主线程就会开始解析数据，生成 `DOM（Document Object Model）` 对象。

`DOM` 是浏览器对页面及其数据结构的表示。通过浏览器提供的 `API`，用户可以获取这些 `DOM` 节点。

浏览器根据 [HTML 标准][3]解析 `HTML` 文档。你可能注意到了，浏览器解析 `HTML` 文档时从不会报错。比如，当缺少结束标签（`</p>`）时，浏览器会自动补上；当发生错误嵌套时，`Hi! <b>I'm <i>Chrome</b>!</i>`（`b` 标签应该在 `i` 标签之后结束）会被浏览器解析成 `Hi! <b>I'm <i>Chrome</i></b><i>!</i>`。这是因为 `HTML` 规范能够优雅地处理这些错误。如果你对这个过程感到好奇，可以阅读 [An introduction to error handling and strange cases in the parser][4]。

### 加载子资源

网页在生成时通常会使用额外的资源（比如图片、`CSS`、`JavaScript`），这些资源文件都会通过网络或缓存加载。当主线程解析数据并构建 `DOM` 树时，会逐一找出并加载这些文件。为了加快页面显示，“预加载扫描器（`preload scanner`）”会同时在后台运行。如果页面中存在 `<img>` 或 `<link>` 之类需要加载资源的标签，当解析器生成相应标签时，预加载器就会通知浏览器进程中的网络线程加载资源。

![主线程解析 HTML 并生成 DOM 树][5]

### 阻塞解析

当解析器碰到 `<script>` 标签时，它会停止解析剩余的 `HTML` 文档，转而加载并执行相应的 `JavaScript` 代码。

为什么？

因为 `JavaScript` 代码能够改变文档结构，比如 `document.write()` 就可以改变整个文档的结构。这就是 `HTML` 解析器需要等待 `JavaScript` 代码执行结束，才能继续解析 `HTML` 的原因。如果想进一步了解 `JavaScript` 执行过程中发生了什么，可以查看 [the V8 team has talks and blog posts on this][6]。

## 提示浏览器如何加载资源

`Web` 开发者可以通过多种方式让浏览器更智能地加载资源。如果 `JavaScript` 代码没有使用 `document.write()`，开发者可以在 `<script>` 标签上添加 [`async`][7] 或 [`defer`][8] 属性，让浏览器异步加载相应资源，而不阻塞解析器执行。在适当的情况下，还可以使用 [JavaScript module][9]，或通过 `<link rel="preload">` 告诉浏览器：该资源会在当前导航中使用，需要尽快加载。你可以阅读 [Resource Prioritization – Getting the Browser to Help You][10] 了解具体情况。

## 样式计算

仅仅有了 `DOM` 树结构，浏览器还不能确定页面如何呈现，因为我们还可以通过 `CSS` 为元素设置更丰富的样式。主线程会解析 `CSS`，并通过 `CSS` 选择器确定每个 `DOM` 节点的样式信息。你可以通过 `DevTools` 查看 `DOM` 节点的样式信息。

![主线程解析 CSS 并应用样式][11]

即使页面没有使用任何 `CSS`，每个 `DOM` 节点也会有默认的样式信息。比如，`H1` 标签就比 `H2` 标签大，每个元素也都有不同的 `margin` 和 `padding`。可以通过 [Chrome 源码][12]查看 `Chrome` 的默认样式信息。

## 布局

渲染进程知道文档的 `DOM` 树和节点的样式信息后，仍然不能将页面呈现在显示屏上。试想一下，如果你通过手机向朋友描述一幅图画：“这里有一个红色的圆，那里有一个蓝色的方块。”这还不足以传递画面的全部内容，因为你的朋友仍然不知道这个圆的具体大小和位置。

![通过电话告诉朋友一幅画][13]

确定 `DOM` 树节点几何信息的过程叫作布局。主线程会遍历所有 `DOM` 节点，根据样式信息进行计算并创建布局树（布局树上的节点拥有坐标信息和几何信息）。但是，`DOM` 中一些不呈现的节点不会出现在布局树上。比如，当一个元素拥有 `display: none` 属性时，它就不属于布局树上的节点。相反，如果某个元素使用了伪元素，比如 `p::before { content: "Hi!"; }`，虽然伪元素并不在 `DOM` 树上，却属于布局树上的节点。

![主线程通过计算 DOM 树和样式信息确定布局树][14]

确定页面布局是一项具有挑战的任务。即使是最简单的布局，比如块级元素从上到下排列，也必须考虑元素内部字体的大小以及如何换行，因为这些都会影响元素的形状和大小，甚至可能影响下一个块级元素的位置。

<video loop muted playsinline controls src="https://pic.breeze.red/t-chrome-blog-layout.mp4"></video>

段落的盒模型因为换行而产生变化。

`CSS` 能够设置元素的浮动和定位，而这些设置又会覆盖其他元素，甚至影响文本的显示方式。所以，布局是一个庞大的工程。在 `Chrome` 中，有一整个团队在解决这个问题。如果想深入了解布局，可以查看 [few talks from BlinkOn Conference][15]。

## 绘画

拥有 `DOM` 树、样式信息和已经生成的布局树，仍不足以渲染出一个页面。如果你想复制一幅画，光知道上面的信息当然不够，还需要知道画中各个元素的绘制顺序，因为后画的元素会覆盖先画的元素。页面的呈现也是如此。

![小女孩确定绘画的先后顺序][16]

比如，某些元素使用了 `z-index` 属性，如果仍然从上到下绘制元素，显然是错误的。

![依次绘画元素导致错误的结果][17]

在绘画这个步骤中，主线程会遍历整个布局树，生成一系列绘制记录。绘制记录描述了绘画步骤，比如：“先画背景，接着画出文字，最后画出一个矩形。”如果你曾经使用过 `canvas` 进行绘画，那么应该会对这个步骤很熟悉。

![主线程通过编辑布局树生成绘画记录][18]

### 更新消耗

需要着重关注的一点是：从 `DOM & CSS` 合成布局树，再由布局树生成绘画记录，是一个连续的过程。在这个过程中，每一步都依赖前一步生成的数据。如果 `DOM` 或 `CSS` 结构发生改变，还需要通过以上步骤重新生成受影响部分的绘制记录。

<video loop muted playsinline controls src="https://pic.breeze.red/t-chrome-blog-trees.mp4"></video>

`DOM` 树和样式信息、布局树、绘制记录按顺序生成。

当网页开发者为元素设置动画时，浏览器需要在每一帧之间执行这些操作。大多数显示屏每秒刷新 `60` 次。如果开发者对每一帧都进行了处理，那么页面效果在用户眼中就会变得平滑、流畅。但是，如果浏览器的某一帧没有及时处理或被忽略，用户看到的效果就会出现卡顿。

![动画效果][19]

即使页面绘制过程可以确保在一帧内完成，但计算代码也运行在主线程中，这就意味着页面绘制可能会被 `JavaScript` 代码阻塞。

![动画顺序执行，但是被 JavaScript 代码所阻塞][20]

为了解决这种情况，开发者可以将代码切割成几个小块，用 `requestAnimationFrame()` 依次调用。想进一步了解，可以查看 [Optimize JavaScript Execution][21]。当然，使用 [`Web Worker`][22] 也是一个不错的选择。

![将代码切割成几小块执行][23]

## 合成

### 如何绘制一个页面

到目前为止，浏览器已经知道了文档结构，以及每个元素的样式信息、几何形状和绘制顺序。那么，它是如何进行绘制的呢？

> 光栅化——将几何信息转换为屏幕像素的过程。

一种简单的方式，就是将视窗内（页面在浏览器上呈现的部分）的内容光栅化。如果用户滚动页面，则移动光栅，并填充缺失的部分。这就是 `Chrome` 首次发布时处理光栅化的方式。但是，现代浏览器采用了更加复杂的处理方式，称为合成。

<video loop muted playsinline controls src="https://pic.breeze.red/t-chrome-blog-naive_rastering.mp4"></video>

使用简单方式处理动画

### 什么是合成

合成就是将页面的各个部分分层，分别进行光栅化，再由单独的线程负责将它们组成一个页面。当滚动行为发生时，由于各层元素已经完成光栅化，合成线程只需要重新组合这些层，生成一个新帧即可。通过移动分层，也可以实现动画效果。

<video loop muted playsinline controls src="https://pic.breeze.red/t-chrome-blog-composit.mp4"></video>

通过合成实现动画

你可以通过 `Chrome` 中的 `DevTools` 了解浏览器具体的分层信息。

## 分层

为了确定页面元素属于哪个分层，主线程会遍历布局树，同时创建分层树。如果确定页面上的某部分应该属于单独一个分层（例如侧滑菜单），你可以通过 `will-change` 属性告知浏览器。

![主线程遍历布局树生成分层树][24]

理想情况下，我们可以给每个元素一个单独的分层。但是，如果需要合并的图层过多，可能会导致合成缓慢，甚至不如不分层。因此，测量页面的呈现性能非常重要。可以阅读 [Stick to Compositor-Only Properties and Manage Layer Count][25]，了解如何提升页面渲染性能。

### 分离栅格化和合成过程

当分层树和绘制顺序确定后，主线程会通知合成线程进行页面合成。合成线程会将每个分层光栅化。一个分层可能和整个页面一样大，因此，合成线程会将分层内容切割成多个区块，再发送给光栅线程。光栅线程处理后，会将数据存储在 `GPU` 内存中。

![光栅线程生成信息传入到 GPU 中][26]

合成线程可以对不同的光栅线程进行优先级排序，视口内（或视口附近）的元素会被优先光栅化。图层还会由多个不同分辨率的图块组成，用来处理放大等操作。

元素完成光栅化后，就会被合成线程收集。当所有元素都完成光栅化后，合成线程就会创建一个合成帧。

合成线程生成合成帧后，会通过 `IPC` 通知浏览器进程。与此同时，另一些来自 `UI` 线程或扩展进程的合成帧也会被发送到 `GPU`，从而展示在屏幕上。当页面触发滚动事件时，合成线程就会生成另一个合成帧，并将其发送到 `GPU`。

![合成线程生成合成帧，并发送到 GPU][27]

这样设置合成线程的原因在于：它不在主线程中执行，因此不需要等待 `DOM` 信息的计算和 `JavaScript` 的执行。这就是为什么[仅合成动画][28]被认为是获得平滑性能的最佳选择。如果元素信息必须通过计算获得，那么就必须等待主线程执行完毕，合成线程才能继续生成合成帧。

## 总结

在这篇文章中，我们了解了页面渲染从解析到合成的步骤。希望你现在有兴趣了解更多关于网站优化的内容。

在下一篇文章中，我们将更加详细地介绍合成线程中发生的事情，并了解当用户触发 `mousemove`、`click` 等事件时会发生什么。

## 相关文章

- [CPU、GPU、内存以及多进程架构](https://breeze.vin/inside-browser-part1/)
- [一次导航到底发生了什么？](https://breeze.vin/inside-browser-part2/)
- [详解渲染进程](https://breeze.vin/inside-browser-part3/)
- [事件合成器](https://breeze.vin/inside-browser-part4/)

[1]: https://developers.google.com/web/fundamentals/performance/why-performance-matters/
[2]: https://pic.breeze.red/post/t-chrome-blog-renderer.png
[3]: https://html.spec.whatwg.org/
[4]: https://html.spec.whatwg.org/multipage/parsing.html#an-introduction-to-error-handling-and-strange-cases-in-the-parser
[5]: https://pic.breeze.red/post/t-chrome-blog-dom.png
[6]: https://mathiasbynens.be/notes/shapes-ics
[7]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attr-async
[8]: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attr-defer
[9]: https://developers.google.com/web/fundamentals/primers/modules
[10]: https://developers.google.com/web/fundamentals/performance/resource-prioritization
[11]: https://pic.breeze.red/post/t-chrome-blog-computedstyle.png
[12]: https://cs.chromium.org/chromium/src/third_party/blink/renderer/core/html/resources/html.css
[13]: https://pic.breeze.red/post/t-chrome-blog-tellgame.png
[14]: https://pic.breeze.red/post/t-chrome-blog-layout.png
[15]: https://www.youtube.com/watch?v=Y5Xa4H2wtVA
[16]: https://pic.breeze.red/post/t-chrome-blog-drawgame.png
[17]: https://pic.breeze.red/post/t-chrome-blog-zindex.png
[18]: https://pic.breeze.red/post/t-chrome-blog-paint.png
[19]: https://pic.breeze.red/post/t-chrome-blog-pagejank1.png
[20]: https://pic.breeze.red/post/t-chrome-blog-pagejank2.png
[21]: https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution
[22]: https://www.youtube.com/watch?v=X57mh8tKkgE
[23]: https://pic.breeze.red/post/t-chrome-blog-raf.png
[24]: https://pic.breeze.red/post/t-chrome-blog-layer.png
[25]: https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count
[26]: https://pic.breeze.red/post/t-chrome-blog-raster.png
[27]: https://pic.breeze.red/post/t-chrome-blog-composit.png
[28]: https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/
