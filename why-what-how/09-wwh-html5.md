---
title: 如何理解 HTML5？
url: /wwh-html5
---

## 前言

作为程序员，技术的落实与巩固是必要的，因此想到写个系列，名为 `why what or how` 每篇文章试图解释清楚一个问题。

这次的 `why what or how` 主题：现在几乎所有人都知道了 `HTML5` ，那么 `H5` 到底相比于 `HTML4` 有什么区别呢？

## 升级版？标准版！

`HTML5` 做为 `HTML` 标准的第 `5` 版，于 `2014` 年发布，相信关注过 `HTML5` 的发展史的朋友都知道该版本是 `WHATWG` 和 `W3C` 握手言和后诞生的，是 `W3C` 组织与浏览器厂商相互妥协的结果，其中的绝大部分规范都由 `WHATWG` 组织所制定，之后由 `W3C` 采纳并入标准中。

但 `HTML5` 是 `HTML4` 的升级版吗？

是也不是，如果以版本的更迭来看，确实，`HTML5` 确实在 `HTML4` 之后诞生，但是 `HTML5` 本质上却已经不同于 `HTML4` 了。

相信大家目前都还留有对 `HTML4` 的些许印象，在写 `HTML4` 时需要加一段特殊的说明来声明文档类型的：

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

该段声明说明了该文档基于 `http://www.w3.org/TR/html4/strict.dtd` 规范，从一个侧面来看，该文档仅仅是符合这种规范的文本而已，而这种规范被称为 `SGML（标准通用标记语言）`。

接着在来看看 `HTML5` 的文档声明：

```html
<!DOCTYPE html>
```

简简单单，清晰明了，说明该文档是 `HTML` 文档，并没有基于规范。

从浏览器的角度来对比，`HTML5` 之前的版本，浏览器必须要从 `W3C` 官网上获取规范文件，才能解析文档，而 `HTML5` 文档可以被浏览器直接识别。

这些角度都表明了，`HTML5` 之前的 `HTML` 是 `SGML` 规范的一种实现，而 `HTML5` 却是原原本本的 `HTML`，有自己的规范，因此 `HTML5` 可以认为是一种新的标准，该规标准诞生于 `HTML4`，却不是 `HTML4` 的升级。

但不管是基于 `SGML` 的 `HTML` 版本，还是 `HTML5` 版本，其语法内容都差不多。因此也不需要过多的纠结在这个问题上，实用主义者，能用好用就行~

## 语义化

在看过 [什么是 CSS？](https://blog.acohome.cn/what-is-css/) 之后，大家应该知道样式的发展过程中经历了一个用标签表达样式的阶段，该阶段产生了一些样式标签，比如 `<font>` `<center>` `<big>` 等。但 `HTML5` 提出结构和样式分离，样式表达在 `css` 内，而 `HTML` 负责结构，样式标签正式宣告脱离舞台，在 `HTML5` 的规范下，这些标签正式被废除，一些代表结构的标签被提出并规范。

结构化，样式脱离的 `HTML` 文档，极大提高了文档的表达力，不管是机器还是人类都能更快的理解文档的结构，这种文档就被称为语义化文档。

语义化主要体现在以下几个方面：

### 结构标签

写过 `HTML5` 的都知道大概有哪些标签新加了进来，这里大致罗列一下

| 标签          | 释义                                                              |
| ---           | ---                                                               |
| `section`     | 代表文档组成的一部分，应该与 `h[1-6]` 结合使用，表示文档结构。    |
| `article`     | 代表文档的独立内容区域，该区域内容为博文、报纸文章等内容。        |
| `main`        | 代表文档的主要内容区域。                                          |
| `aside`       | 代表与内容主题略有相关的区域。                                    |
| `header`      | 代表该文档的介绍性区域。                                          |
| `footer`      | 代表该文档的页脚，通常用于存放版权，作者等信息。                  |
| `nav`         | 代表文章导航区域。                                                |
| `figure`      | 代表该区域内有多媒体内容，通常多媒体标签作为该标签的子元素。      |
| `template`    | 模板区域，可以被 `JavaScript` 所使用。                            |
| `video`       | 通过该标签可以引用视频，该标签代表视频可以显示的区域。            |
| `audio`       | 通过该标签可以引用音频，该标签代表音频可以显示的区域。            |
| `track`       | 该标签为 `video` 提供了字幕。                                     |
| `mark`        | 代表需要特殊注意的内容区域。                                      |
| `progress`    | 该区域代表某项任务的进度。                                        |
| `meter`       | 该区域代表某项内容的使用量，比如 CPU 使用量等。                   |
| `time`        | 该区域代表时间。                                                  |
| `canvas`      | 该区域代表可以被 `JavaScript` 所控制的画布。                      |

可能有些朋友会纳闷，这些东西其实吧，也不是那么的重要，用 `div + css` 也都能实现，而且各个浏览器上表现的还都一致，还不用考虑兼容问题。那么为什么这里还提呢？

试想一个问题？

如果你是浏览器，或者说是解析 `HTML5` 文本内容的程序，你会怎么看待 `HTML4` 与 `HTML5` ？

首先第一个需要解决的问题：显示页面，两者都能解析，生成树状结构，并成功的显示页面，这也是大部分用户所需要的。在这方面两者平局，`HTML5` 没有任何的优势。

那么现在第二个问题，识别该文档，这或许不是大部分用户的需求，但小部分用户并不意味着人数少。

相信大部分朋友都有使用搜索引擎的习惯，那么搜索引擎是如何帮你找到这个网页的呢？

搜索引擎通过适当的策略解析通过爬虫抓取到的网页内容，将该文章进行分类，通过用户给的搜索词进行匹配，最终将结果呈现给用户，爬虫的解析策略，面对 `HTML4` 时，需要识别出特定的 `div` 而这个特定的 `div` 该如何去找呢？经验？特定字符串？面对 `HTML4` ，只能得到一个个的 `div` 而哪个 `div` ，emmm...  不好找。面对 `HTML5` 呢？程序从结构就能大致理解文档的结构，进而识别出该篇文章的大体内容。

问题二可以简单认为：我们需要生成文章大纲，一篇文章可以有几部分的内容组成，而规范这些内容的就是大纲，相信有写过文章的都知道大纲的作用，可以清醒的表达出内容，那么 `HTML4` 呢，一堆 `div` 分不清哪个是主体，哪个是次要内容，但 `HTML5` 呢，结构清晰，解析程序也能很快的生成文档的大纲。

大纲只能用在搜索引擎吗？并不是，阅读设备就很需要大纲生成目录，无障碍设备可以识别到文档的主体内容等等。

看到这里，相信大家对于语义化标签的重要性也有了了解，使用成本几乎为 `0`，但文档的清晰程度却上了一个层级，干嘛不用呢？

### 新增的表单类型

表单类型（`type`）代表了改表单内容的类型，新增了以下几个

- `tel`
- `search`
- `url`
- `email`
- `date`
- `time`
- `number`
- `range`
- `color`

### 新的标签属性

属性                  | 示例                        | 含义
---                   | ---                         | ---
`target`              | `<base target="...">`       | `base` 标签新增 `target`属性，虽然之前已经被广泛的使用了。
`charset`             | `<meta charset="utf-8">`    | `meta` 标签新增 `charset` 属性，规范文档所使用的字符集。
`placeholder`         | `<input placeholder="...">` | 可输入的表单元素新增 `placeholder` 属性，代表默认内容。
`form`                | `<input form=1>`            | 表单元素新增 `form` 属性，代表关联的 `form` 表单，而不需要在 `form` 标签内。
`contenteditable`     | `<div contenteditable>`     | 代表该元素可编辑。
`data-*`              | `<div data-a="...">`        | 与该元素相关联的数据，可在 `JavaScript` 中获取。

以上为 `HTML5` 语义化的具体呈现，希望大家拥抱并使用 `HTML5` 的新标签。

## 多媒体

`HTML5` 的上一代标准 `HTML4` 于 `1999` 年发布，当时互联网还处于图文混排的时代，通讯也不是很发达，数据的交互受限于网速，但是近 `20` 年来数据通讯的高速发展图文混排已经不能满足用户的需求了，由于浏览器并不支持视频促成了 `Flash/ActionScript` 的发展，但是由于 `Flash` 的繁琐（需要装插件）以及安全问题，`Apple` 拒绝使用 `Flash`，之后各大浏览器厂商也开始重视 `Flash` 的安全问题，提出了多媒体标签，直到 `HTML5` 规范化了多媒体应用。

相关多媒体标签

- [Audio](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio)
- [Video](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video)

建议查看英文的相关文档，中文的翻译文档好像还没有及时跟进。

除了多媒体标签的更新，这里同时还列举下一些与显示相关的内容：

- Canvas

画布，可以使用 `JavaScript` 在改元素规定的区域内进行绘画，[文档说明](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial)。

- WebGL

光栅化的图片绘画引擎，主要用于 `3D` 制作，[文档说明](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API)，以及[WebGL 理论基础](https://webglfundamentals.org/webgl/lessons/zh_cn/)

- Camera API

通过[Camera API](https://developer.mozilla.org/zh-CN/docs/Web/Guide/API/Camera)，你可以使用手机的摄像头拍照，然后把拍到的照片发送给当前网页。

- SVG

现在可以直接在 `HTML5` 文档中写 `SVG`（可缩放矢量图形），一种用于描述基于二维的矢量图形的，基于 `XML` 的标记语言。[文档说明](https://developer.mozilla.org/zh-CN/docs/Web/SVG)。

- MathML

`MathML（Mathematical Markup Language）`，是一种基于 `XML` 的标记语言，用以描述数学公式，但目前浏览器的支持力度有点低。具体可以查看[文档说明](https://developer.mozilla.org/zh-CN/docs/Web/MathML)

## 通信

`HTML5` 之前，`JavaScript` 只能通过 `XMLHttpRequest` 与后端程序进行交互，随着 `HTML5` 标准的提出，前端可以使用各种各样的姿势与后端进行沟通，数据的交互变的简单，灵活。

### fetch

`XMLHttpRequest`，虽然在实际开发中几乎很难使用到它，但是对于它的封装库我们却经常使用，比如 `axios` `$.ajax` 。虽然说封装的库降低使用成本，但源头是复杂的，而且 `XMLHttpRequest` 是基于回调的异步模式，虽然 `axios` 进行了 `Promise` 化，但毕竟不是原生支持。因此一个基于 `Promise` 的请求 `API` 于 `HTML5` 标准内提出：`fetch`。

一个简单的 `fetch` 例子

```javascript
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```

`fetch` 支持两个参数，

1. 资源地址
2. 请求参数，用于设置请求方式，请求头，请求体等内容，一个 JavaScript 对象。

学习资源：

- MDN [使用 Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)
- WHATWG [Fetch](https://fetch.spec.whatwg.org/#fetch-api)

### WebSocket

在 `HTML5` 出现之前，做一个实时聊天的 `web` 项目是极其困难的，由于之前的版本仅支持浏览器主动向服务器进行发送内容，必须先有请求才能有服务器响应的数据，简单的来说服务器并不能主动将数据提交给客户端。

面对这些需求，大家的做法通常是轮询，通过前端设置一个定时器，每隔固定的时间去请求服务器资源，即使服务器并没有发生数据更新。这浪费了极大的服务器资源，那么有没有一种方式支持服务器推送呢？

有！`WebSocket` 是 `HTML5` 新增的一种协议，目的是在浏览器和服务器之间建立一个双向数据通道，浏览器可以推送数据到服务端，服务端也可以推送数据到客户端。`WebSocket` 对于前端来说仅是一个简单的 `API`，最重要的是服务端的支持。

`WebSocket` 的客户端代码示例

```javascript
// 创建链接
const socket = new WebSocket('ws://localhost:8080');

// 链接开启事件
socket.addEventListener('open', function (event) {
    // 向服务端推送数据
    socket.send('Hello Server!');
});

// 服务端推送事件，event.data 即为服务端推送的数据
socket.addEventListener('message', function (event) {
    console.log('Message from server ', event.data);
});
```

至于服务端如何实现，不同的语言有不同的实现，比较出名的是 `Node` 下的 `Socket.IO`，如果担心服务端语言实现困难，不妨用 `Node` 搭建中间层，使用 `Node` 进行与客户端的沟通，并将数据交由服务端程序处理。当然也可以查看 `MDN` 的文章：[编写 WebSocket 服务器](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSockets_API/Writing_WebSocket_servers)

一些资源

- WHATWG [Web sockets](https://html.spec.whatwg.org/multipage/web-sockets.html#websocket)
- websocket.org [HTML5 WebSocket](http://www.websocket.org/quantum.html)
- MDN [WebSockets](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSockets_API)

### EventSource

`fetch` 实现了请求的 `Promise` 化，`WebSocket` 实现了双工通信，那么 `EventSource` 又是什么？

简单的来说，`EventSource` 实现了服务端对客户端的单向通信，可以理解为客户端是订阅者，服务端是内容发布者，也就是发布订阅模式的实现。

一个简单的例子

```javascript
// 开始订阅
var evtSource = new EventSource('sse.php');
var eventList = document.querySelector('ul');

// 服务端发布数据回调
evtSource.onmessage = function(event) {
    // event.data 即为服务端发布的数据
    console.log('Message from server ', event.data);
}
```

同样的前端代码很简单，服务端需要实现该通信方式需要有一定的成本。具体可以查看一下文档

- WHATWG [EventSource](https://html.spec.whatwg.org/multipage/server-sent-events.html#the-eventsource-interface)
- MDN [EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)
- [see.php 编写](https://github.com/mdn/dom-examples/blob/master/server-sent-events/sse.php)

### WebRTC

`WebRTC`（网页即时通信），是一个支持网页浏览器进行实时语音对话或视频对话的 `API`。由于该 `API` 目前并未纳入标准中，这里就不过多的深入了，可以通过以下文档进行了解：

- MDN [WebRTC API](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API)
- W3C [WebRTC 1.0](https://w3c.github.io/webrtc-pc/)

## 存储

在 `HTML5` 出现之前，前端开发者仅能通过 `cookie` 来存储用户在不同页面之间跳转的数据，但使用 `cookie` 来存储数据其意义上是不对的，不能说 `cookie` 可以在同域名下通用，就让它来携带各种各样的数据，`cookie` 应该是用来标志用户身份的。`HTML5` 出现之后，规范了数据的存储，`cookie` 也回到了原始的用途。

### storage

相信大家对于该 `API` 已经熟悉的不能在熟悉了，`storage` 分为两部分，`sessionStorage` 与 `localStorage`，拥有同样的方法：

- `setItem`
- `getItem`
- `removeItem`
- `clear`

`localStorage` 会将数据永久的存储下来（当然用户可以主动清楚），`sessionStorage` 中设置的数据当用户关闭页面后会清除。

- WHATWG [Web storage](https://html.spec.whatwg.org/multipage/webstorage.html#the-storage-interface)
- MDN [Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)

### IndexedDB

小型的数据有 `storage` 已经够用，那么浏览器端如何进行大量数据的存储呢？`IndexedDB` 就是 `HTML5` 给出的答案。`IndexedDB` 是浏览器内置的一个数据库，用于存储结构化数据。但这个数据库是异步存取数据的，说实话有点难使用，如果使用他，那基本上所有的代码都在 `then` 方法里了。考虑使用吧。

可以通过[使用 IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API/Using_IndexedDB)进行学习和使用。

## DOM 的更新

### query

- `querySelector`         使用 `CSS` 选择器进行选择元素，仅选取匹配元素中的第一个。
- `querySelectorAll`      使用 `CSS` 选择器进行选择元素，返回所有匹配的元素。

### data-*

可以通过在标签上使用 `data-*` 设置与元素绑定的数据，`JavaScript` 可以通过 `dataset` 获取或设置。

```html
<p id="title" data-text="hello"></p>
```

```javascript
const p = document.querySelector('#title');
p.dataset.text // hello
```

### 元素添加

- `prepend`   在规定标签的最前面添加元素
- `append`    在规定标签最后面添加元素

### element 相关

- `firstElementChild`         获取规定标签中的第一个子元素
- `lastElementChild`          获取规定标签中的最后一个子元素
- `previousElementSibling`    获取前一个兄弟元素
- `nextElementSibling`        获取后一个兄弟元素

### 类名操作

- `Node.classList.add`        添加类名
- `Node.classList.remove`     移除类名
- `Node.classList.toggle`     切换类名
- `Node.classList.contains`   检测是否有类名

### 拖拽

在 `HTML4` 中想实现元素拖拽，我们需要这样一个过程，获取元素，设置元素定位，与需要被拖入的元素进行碰撞检测，检测完成后进行拖入操作。在 `HTML5` 中规范了这一过程。

相关事件

- `dragstart`     设置在拖动元素上，标志拖动开始
- `dragenter`     设置在拖入元素上，标志有元素开始拖入到该元素内
- `dragover`      设置在拖入元素上，标志元素完全拖入该元素
- `dragleave`     设置在拖入元素上，标志元素即将离开该元素
- `drop`          设置在拖入元素上，标志拖动元素放下

拖动元素与拖入元素可以通过 `e.dataTransfer.getData(xxx)` `e.dataTransfer.setData(xxx, xxx)` 进行数据交互。

### MutationObserver

`MutationObserver` 字面意思为变动观察器，用来观察文档节点的变动。相关文章

- [了解 HTML5 中的 MutationObserver](https://segmentfault.com/a/1190000012787829)
- [MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)

### 最后

`DOM` 中，接触最多的也就这些内容了，更为详细的内容可以通过[DOM STANDARD](https://dom.spec.whatwg.org/)查看。

## BOM 的新增

在[什么是 JAVASCRIPT](https://blog.acohome.cn/what-is-javascript/)中，我们提到过 `BOM` 并没有相关的标准去规范它，`HTML5` 中一些没有归到 `DOM` 中 `API` 更新就归到这。

### Web Workers

我们都知道 `JavaScript` 是以单线程的形式运行在客户端，这就避免不了当进行大规模的数据运算时所带来的堵塞问题，为解决这一问题 `HTML5` 提出了 `Web Workers` 的概念，并最终纳入规范。使用 `Web Workers` 可以将于页面渲染无关的数据运算代码放入另一个线程，线程间通过 `postMessage` 发送数据，通过 `onmessage` 回调获取数据。

一个简单的例子

```javascript
// workers.js
let i = 0;

function timedCount() {
    i = i + 1;
    postMessage(i);
    setTimeout(timedCount, 500);
}
timedCount();
```

```javascript
const w = new Worker("workers.js");
w.onmessage=function(event){
    console.log('Message from worker ', event.data);
};
```

相关资源：

- WHATWG [Worker](https://html.spec.whatwg.org/multipage/workers.html#workers)
- MDN [Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker/Worker)

目前大火的 `PWA` 应用，其核心 `Service Worker` 其实也是 `Web Workers` 的一个实现。 `Service Worker` 使用编程的方式来进行资源的缓存，与后台进行数据同步等内容。虽然该内容已在不少的浏览器上实现，但标准仍在草案阶段，等出标准规范在详细介绍。

相关资源

- W3C [Service Workers](https://w3c.github.io/ServiceWorker/)

### History

在 `HTML5` 之前，前端开发者只能通过 `history` 对象进行前进后退的动作，但却不能在不刷新当前页面的情况下进行 `url` 修改，在 `HTML5` 之前，单页应用仅能通过 `hash` 模式进行路由管理，但通过 `HTML5` 新提出的 `history API` ，单页应用就能像正常应用那样管理页面的 `url` 了。通过以下 `3` 个方法即可：

- `pushState`         不刷新页面导航到指定 `URL` 并产生历史记录。
- `replaceState`      不刷新页面并将页面当前 `URL` 替换为指定 `URL`，不产生历史记录。
- `popstate` 事件     在当前页面点击返回时，window 对象会触发该事件，可以使用 addEventListener 进行监听。

相关资源

- MDN [Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)
- WHATWG [Drag and drop](https://html.spec.whatwg.org/multipage/dnd.html#dnd)
- [Drag Drop---API---拖拽事件](https://www.jianshu.com/p/6116b71552f8)

### requestAnimationFrame

通知浏览器，在下一次重绘之前调用指定 `callback`。由于重绘与屏幕刷新率挂钩，因此该 `API` 的调用也与屏幕刷新率挂钩，可以使用该 `API` 进行一些复杂运算的分解，将任务放在每次刷新的间隔中，这样就不会产生屏幕要刷新了但浏览器却能及时绘制出页面的尴尬情况出现，使得页面显示平滑，不掉帧。因此该 `API` 的出现促进了 `Canvas` 的发展，而 `Canvas` 的发展又促进了页游的发展。

- `requestAnimationFrame(callback)`

如果想实现类似定时器的效果，只需要在 `callback` 中再次调用 `requestAnimationFrame` 即可。

相关资源

- MDN [requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)

### 全屏 API

- `Element.requestFullscreen()`   将当前元素全屏展示
- `Document.exitFullscreen()`     取消全屏显示

相关资源

- MDN [全屏 API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fullscreen_API)

### 获取地理位置信息

- `navigator.geolocation.watchPosition()`         监听地理位置信息，当发生改变时，触发回调
- `navigator.geolocation.getCurrentPosition()`    获取地理位置信息

以上两方法都需要传入，成功回调和失败回调，分别为参一和参二。

相关资源

- MDN [使用地理位置定位](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/Using_geolocation)

## CSS3

在[什么是 CSS？](https://blog.acohome.cn/what-is-css/)中，我们提到过，`CSS` 标准被 `W3C` 采纳，并最终成为 `HTML` 标准的一部分，因此 `CSS` 内容的更新也算是 `HTML5` 更新的一部分。

这部分单独说吧。

## 总结

惯例，问几个问题

- `HTML5` 涵盖了那几方便面的更新？
- 通信方式有什么新姿势？
- 语义化的体现，以及语义化的作用？
- `js` 如何更好的处理数据而不影响页面渲染？
- 单页应用的 `history` 是基于那些 `api` 实现的？

谈谈感受

随着 `HTML5` 的更新，前端在也不是切图仔，浏览器也开始做为了一个 `GUI` 工具存在，而不仅是为了展示页面。相信随着技术的发展，当通讯延迟降低到一定程度后，乔帮主的设想 `all in browser` 将会成为现实。

## 参考

- [HTML5](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5)
- [HTML5 Differences from HTML4](https://www.w3.org/TR/html5-diff/)
- [HTML 5.1 2nd Edition](https://www.w3.org/TR/html51/)
- [HTML 5.2](https://www.w3.org/TR/html52/)
- [WHATWG](https://spec.whatwg.org/)
