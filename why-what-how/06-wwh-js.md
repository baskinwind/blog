---
title: 什么是 JavaScript？
url: /wwh-js
excerpt: JavaScript 能操作页面，也能跑在服务器，甚至还能写桌面应用——这些能力到底属于语言，还是宿主环境？先把边界划清，再重新回答：什么是 JavaScript？
---

## 前言

作为程序员，技术的落实与巩固是必要的，因此想到写个系列，名为 `why what or how`，每篇文章试图解释清楚一个问题。

这次的 `why what or how` 主题：什么是 `JavaScript`？

## 释义

`JavaScript`：一种解释性脚本语言。

> 解释性脚本语言：一类不具备开发操作系统的能力，而是只用来编写控制其他大型应用程序的“脚本”，但其内容的执行不需要提前编译的语言。通常作为别的程序的输入。

`JavaScript` 是一种用于描述网页逻辑、处理网页业务的解释性脚本语言。它使用纯文本，其内容作为浏览器的输入，由浏览器负责解释、编译和运行。

目前，`JavaScript` 也成功应用于服务端。服务端的 `JavaScript` 用于描述业务逻辑，其内容作为 `Node.js` 的输入，由 `Node.js` 负责解释、编译和运行。

`JavaScript` 是伴随浏览器出现的一门特殊语言，特殊在哪呢？`JavaScript` 是唯一一门所有浏览器都支持的脚本语言。也就是说，如果你的 Web 程序想在客户端做点事，就一定会用到 `JavaScript`。别看现在 `JavaScript` 炙手可热，它最开始出现时却仅用于验证表单。

## 历史

- `1990 - 1994`，虽然各种浏览器开始出现，但浏览器仅用作数据展示，并没有客户端逻辑存在。
- `1994`，`Netscape` 公司计划实现一种浏览器脚本语言进行一些简单的表单验证。
- `1995`，`Netscape` 公司雇佣了程序员 `Brendan Eich` 开发这种网页脚本语言。
- `1995.5`，`Brendan Eich` 用了 `10` 天，设计完成了这种语言的第一版。取名为：`Mocha`。
- `1995.9`，`Netscape` 公司将该语言改名为 `LiveScript`。
- `1995.12`，`Netscape` 与 `Sun` 公司联合发布了 `JavaScript` 语言。
- `1996.3`，`Navigator 2.0` 浏览器正式内置了 `JavaScript` 脚本语言。
- `1996.8`，微软发布 `Internet Explorer 3.0`，同时发布了 `JScript`，该语言模仿同年发布的 `JavaScript`。
- `1996.11`，`Netscape` 公司在浏览器对抗中没落，将 `JavaScript` 提交给国际标准化组织 `ECMA`，希望 `JavaScript` 能够成为国际标准，以此抵抗微软。
- `1997.7`，`ECMAScript 1.0` 发布。`ECMAScript` 是一种标准，而 `JavaScript` 是该标准的一种实现。
- `1997.10`，`Internet Explorer 4.0` 发布，其中的 `JScript` 基于 `ECMAScript 1.0` 实现。
- `1999`，`IE 5` 部署了 `XMLHttpRequest` 接口，允许 `JavaScript` 发出 `HTTP` 请求，为后来 `Ajax` 应用创造了条件。
- `1999.12`，`ECMAScript 3.0` 版发布，得到了浏览器厂商的广泛支持。
- `2005`，`Ajax` 方法（`Asynchronous JavaScript and XML`）正式诞生，`Google Maps` 项目大量采用该方法，促成了 `Web 2.0` 时代的来临。
- `2006`，`jQuery` 函数库诞生，作者为 `John Resig`。`jQuery` 统一了不同浏览器操作 `DOM` 的不同实现，被广泛使用，极大降低了 `JavaScript` 语言的应用成本，推动了语言的流行。
- `2007.10`，`ECMAScript 4.0` 草案发布，对 `3.0` 版做了大幅升级，但由于改动幅度过大，遭到了各大浏览器厂商的反对。
- `2008.7`，各大厂商对 `ECMAScript 4.0` 版本的开发分歧太大，`ECMA` 开会决定，中止 `ECMAScript 4.0` 的开发，对其中一些已经改善的部分发布为 `ECMAScript 3.1`。不久之后就改名为 `ECMAScript 5`。
- `2008`，由 `Google` 开发的 `V8` 编译器诞生，极大地提高了 `JavaScript` 的性能，为之后 `Node.js` 的诞生打下了基础。
- `2009`，`Node.js` 项目诞生，创始人为 `Ryan Dahl`，`JavaScript` 正式应用于服务端，以其极高的并发进入人们的视野。
- `2009.12`，`ECMAScript 5.0` 正式发布。同时将标准的设想定名为 `JavaScript.next` 和 `JavaScript.next.next` 两类。
- `2010`，`NPM` 和 `RequireJS` 出现，标志着 `JavaScript` 进入模块化。
- `2011.6`，`ECMAScript 5.1` 发布，并且成为 `ISO` 国际标准（`ISO/IEC 16262:2011`）。
- `2012`，单页面应用程序框架（`single-page app framework`）开始崛起，`AngularJS` 项目出现。
- `2012`，所有主要浏览器都支持 `ECMAScript 5.1` 的全部功能。
- `2012`，微软发布 `TypeScript` 语言。为 `JavaScript` 添加了类型系统。
- `2013`，`ECMA` 正式推出 `JSON` 的国际标准，这意味着 `JSON` 格式已经变得与 `XML` 格式一样重要和正式了。
- `2013.2`，`Grunt.js` 前端构建化工具发布，前端进入自动化开发阶段。
- `2013.5`，`Facebook` 发布 `UI` 框架库 `React`。
- `2013.8`，`Gulp.js 3.0` 前端构建工具发布，`JavaScript` 自动化开发变得简单起来。
- `2014`，尤雨溪发布 `Vue` 项目。
- `2015.3`，`Facebook` 公司发布了 `React Native` 项目，将 `React` 框架移植到了手机端，用来开发手机 `App`。
- `2015.3`，`Babel 5.0` 发布，`ES6` 代码正式应用于开发，而不需要考虑兼容问题。
- `2015.6`，`ECMAScript 6` 正式发布，并且更名为 `ECMAScript 2015`。
- `2015.6`，`Mozilla` 在 `asm.js` 的基础上发布 `WebAssembly` 项目。
- `2015.9`，`Webpack 1.0` 发布，模块系统得到广泛的使用。
- `2016.6`，`ECMAScript 2016（ES7）` 标准发布。
- `2017.6`，`ECMAScript 2017（ES8）` 标准发布，引入了 `async` 函数，使得异步操作的写法出现了根本的变化。
- `2017.11`，所有主流浏览器全部支持 `WebAssembly`，这意味着任何语言都可以编译成 `JavaScript`，在浏览器中运行。
- `2018.6`，`ECMAScript 2018（ES9）` 标准发布。
- `2019.6`，`ECMAScript 2019（ES10）` 标准发布。
- ...

纵观发展历史，很容易发现其中几个关键点的出现，极大地促进了 `JavaScript` 的发展。

1. `XMLHttpRequest` 接口出现后，`JavaScript` 开始拥有与服务器沟通的能力，诞生了 `Ajax`，同时也促进了 `RESTful API` 的发展。
2. `jQuery` 函数库诞生，极大地降低了网页开发成本。
3. `V8` 的出现，降低了 `JavaScript` 消耗的资源，加快了编译速度，复杂的 `JavaScript` 程序开始出现。
4. `Node.js` 项目诞生，`JavaScript` 开始应用在服务端。
5. `NPM` 和 `RequireJS` 出现，`JavaScript` 正式进入模块化。
6. `Grunt`、`Gulp`、`Webpack` 等自动化构建工具的出现，极大地降低了开发成本，统一了各种内容的构建方式。
7. `Babel` 的出现彻底让前端程序员放开了手脚，毫无顾忌的使用 `ES6` 而无须担心平台的兼容性问题。
8. `Angular`、`React`、`Vue` 的出现，进一步降低了网页开发成本，单页应用开始出现。
9. `ECMAScript 6` 标准的公布以及实现，弥补了 `JavaScript` 的语言缺陷，`JavaScript` 更好用了。

直到目前，`JavaScript` 已经成为 `GitHub` 上最热门的语言。从前端到后端，再到桌面，都有 `JavaScript` 的身影。但 `JavaScript` 语言本身的内容并不多，那么它究竟包含哪些内容？

## 语法 & 标准库

关于 `JavaScript` 的语法，这里仅罗列一些概念，具体内容可以查看 [JavaScript 开发文档](https://devdocs.io/javascript/)。

### 数据类型

在 `JavaScript` 中所有的数据都有类型，如下所示：

| 类型          | 定义              | 示例                  | 含义                                      |
| ---           | ---               | ---                   | ---                                       |
| `null`        | 空值              | `let a = null`        | 一个特殊的空值                            |
| `undefined`   | 该值未定义        | `let a`               | 该值声明了但未定义                        |
| `Number`      | 数值              | `let a = 1`           | 包括整数、浮点数、科学计数等所有数字      |
| `String`      | 字符串            | `let a = '1'`         | 单个字符也是字符串                        |
| `Boolean`     | 布尔值            | `let a = true`        | 仅有 `true` 和 `false`，代表真假          |
| `Symbol`      | 符号              | `let a = Symbol(42)`  | 一类永远都不会相同的值，常用作对象的 `key`|
| `Object`      | 对象              | `let a = { a: 1 }`    | 一类拥有多个键值对的数据                  |

在 `JavaScript` 中，数据类型除了可以按照上述方式分类，还可以简单地分为：基本数据类型（除了 `Object` 的其他类型）和引用类型（`Object`）。这和变量的存储方式有关，这里不过多深入。那么，`JavaScript` 数据是如何存储的？请查看：[JS 变量存储？栈 & 堆？NONONO！](/wwh-js-memory)

### 运算符 & 流程 & 声明

#### 运算符

分类        | 包含
---:        | ---
算术        | `+` `-` `*` `/` `**` `%` `++` `--` 
比较        | `>` `>=` `<` `<=` `==` `!=` `===` `!==`
布尔运算    | `!` `||` `&&` `?:`
二进制运算  | `|` `&` `~` `^` `<<` `>>` `>>>`

在进行算术、比较、布尔运算时，会出现类型转换，会有一些怪异的表现，那么 `JavaScript` 是如何进行类型转换的？之后会写，请持续关注~

#### 流程

1. `for(let i = 0; i < n; i++){ ... }`
2. `for(let key in obj){ ... }`
3. `for(let value of array){ ... }`
4. `while(true){ ... }`
5. `do{ ... }while(true)`
6. `if(true){ ... }else if(true){ ... }else{ ... }`
7. `switch(a){ case() ... }`

和大多数的语言一样，都有类似的流程语句。仅需要注意 `for...of` 循环即可。

#### 声明

分类            | 包含
---:            | ---
常量            | `const`
变量            | `let` / `var`（不建议使用）
同步函数        | `function` `() => {}`
异步函数        | `async function` `async () => {}`
Generator 函数  | `function*`

OK，`JavaScript` 的基础知识差不多就是这些。这里只是罗列，并不过多深入，有兴趣或不了解的内容，可以通过 [JavaScript 教程](https://wangdoc.com/javascript/index.html)进行学习。

### 标准库

何为标准库？

在了解 `JavaScript` 的发展史后，我们都知道，`ECMAScript` 是 `JavaScript` 语言实现的标准。标准除了规定基本语法外，还定义了一系列 `JavaScript` 执行环境应该提供的对象或函数。这些与语法无关的其他内容，就被称为标准库。标准库主要包含以下内容（一些已废弃或不推荐使用的内容就不罗列了）：

#### 全局函数

1. [eval()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval)
2. [isFinite()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isFinite)
3. [isNaN()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)
4. [parseFloat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)
5. [parseInt()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
6. [decodeURI()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)
7. [decodeURIComponent()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent)
8. [encodeURI()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)
9. [encodeURIComponent()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)
10. [...](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

#### 全局对象

1. [Number](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)
2. [String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String)
3. [Boolean](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Boolean)
4. [Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
5. [Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)
6. [Function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Function)
7. [Error](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error)
8. [Math](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)
9. [Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Date)
10. [RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp)
11. [Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array)
12. [Map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)
13. [Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)
14. [JSON](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON)
15. [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
16. [Generator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator)
17. [GeneratorFunction](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/GeneratorFunction)
18. [AsyncFunction](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/AsyncFunction)
19. [Proxy](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)
20. [Reflect](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect)
21. [WebAssembly](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly)
22. [...](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

具体内容可以点击查看。这里只是罗列一些常用的函数和对象；还有一些对象，如果不操作视频、音频、图像等内容也用不到，这里就不写了。

## 宿主环境

`JavaScript` 作为一门解释性脚本语言，其内容只能作为其他程序的输入，而这个程序就是宿主环境。那么，宿主环境为 `JavaScript` 提供了什么呢？

1. 语法支持：宿主环境最重要的就是需要知道 `JavaScript` 文本内容到底做了什么。
2. 标准库支持：`JavaScript` 代码会使用 `ECMAScript` 所规定的标准库，因此必须实现。
3. 环境实现：不同的宿主环境为了实现不同的功能，会提供不同的实现，比如浏览器中的 `BOM` 和 `DOM`，以及 `Node.js` 中的 `fs`、`path` 等模块。

### 浏览器

作为 `JavaScript` 最重要的宿主环境，浏览器与 `JavaScript` 携手走过了将近 `20` 年。浏览器因为 `JavaScript` 而大放异彩，`JavaScript` 也因为浏览器崭露锋芒。

浏览器除了为 `JavaScript` 提供语法和标准库支持外，还实现了两大类 `API`：`DOM（Document Object Model）` 和 `BOM（Browser Object Model）`。

#### DOM

`DOM（Document Object Model）`——文档对象模型。

不同于 `ECMAScript`，`DOM` 的规范化组织为 `W3C`，也就是说，`DOM` 其实是 `HTML` 标准化的一部分。因此，`HTML5` 的最新标准涵盖一些 `DOM API` 是正常的。可以查看 `W3C DOM` 的最新规范：[Document Object Model](https://www.w3.org/DOM/)。

那么何为文档对象模型？

我们都知道，`HTML` 可以简单地理解为标签的嵌套，嵌套的标签形成了树状结构。`DOM` 将该树状结构提炼成了 `JavaScript` 中的一个对象：`document`。通过该对象，`JavaScript` 就拥有了操作文档中某个标签的能力，从而改变标签的结构、样式和内容，监听标签的事件等等。

`DOM` 涉及的 `API` 很多，但其 `API` 都有一个特点：以“标签”打头，比如 `body.addEventListener`、`document.getElementsByTagName` 等等。可以通过以下内容进行学习：

- [什么是 DOM?](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model/Introduction)
- [DOM 概述](https://wangdoc.com/javascript/dom/general.html)

#### BOM

`BOM（Browser Object Model）`——浏览器对象模型。

`BOM` 其实到目前还没有一个规范化组织来制定标准，因此，各个浏览器完全按照自己的标准来实现。`MDN` 上也没有专门的介绍页面，说实话有点惨。不过，虽然没有标准，各个浏览器实现的 `API` 却基本相同。

`BOM` 可以简单地理解为：浏览器实现了一套 `API`，使得 `JavaScript` 能与浏览器进行交互。相关的 `API` 全部放在 `window` 对象下。

`window` 对象主要包含以下对象：

主要对象名          | 含义
---:                | ---
`document`          | 对 `DOM` 的引用
`frames`            | 对当前页面中的 `iframe` 的引用
`history`           | 浏览器浏览记录相关 `API`
`location`          | 浏览器地址相关 `API`
`localStorage`      | 本地数据存储
`sessionStorage`    | 会话级别的数据存储
`navigator`         | 浏览器导航信息相关 `API`
`screen`            | 屏幕信息

### Node.js

`Node.js` 作为 `JavaScript` 服务端的宿主环境，除了提供语法和标准库的支持外，还实现了一系列模块，每个模块都有具体的作用，具体可以查看 [Node.js 官方文档](https://nodejs.org/dist/latest-v10.x/docs/api/)。

其实异步的编程模式是不容易被应用在服务端的，或者说服务端更偏向于同步模式。

服务端的绝大多数代码都需要运行在一个同步的环境下，比如数据库的查找、文件的读取、请求结果的读取等等。如果将逻辑都写在异步回调中，代码将变得难以解读，编写起来也会更加复杂。比如，需要按照顺序读取 `10` 次外部接口的数据，同步模式下只要按照顺序从前往后写即可，而异步模式只能嵌套再嵌套（或者使用 `Promise.then`），不仅写出来的代码难以读懂，也难以维护。

但是，为什么 `Node.js` 还是大热呢？个人感觉有以下几个原因：

1. 前端自动化打包的出现。
2. 虽然异步模式不适合服务端，但却极其符合请求的过程：请求触发任务。
3. 主线程仅分发任务，而不需要处理读取文件、数据库等耗时逻辑，不会导致程序阻塞，并发量可以达到很高。
4. 其数据结构与前端需要的结构一致（原生支持 `JSON`）。
5. 语法灵活，直接操作数据，屏蔽或是添加一些字段，处理一些前置逻辑。
6. 简单，包括环境搭建简单、启动简单、程序编写简单，还有 `NPM` 上的各种库。
7. `ES8 AsyncFunction` 异步函数的出现，将异步模式的写法向同步写法趋近。代码编写变得简单起来。
8. 前端人直接上手？

对于第 `8` 点，我持中立态度。大前端的概念层出不穷，我想大家需要冷静冷静，不妨上手写个 `Node.js` 程序，感受一下前端异步编程是否真正适合服务端。还有，服务端虽然环境统一，但涉及的概念极多。虽然在自己写的小项目中可能不会涉及，但真正使用时却会用到。所以，对于 `Node.js` 能做什么，我的理解如下：

1. 作为前端开发的自动化工具，如 `webpack`、`gulp` 等。语法一致，都能看懂，无非是增加了一些文件读取、字符串解析。
2. 爬虫，抓取一些自己感兴趣的东西。对于自己的小程序，可以放开手做，数据量不大，怎么整都行。
3. 数据预处理。在大型项目中，作为请求中转站的存在，返回更加适合前端的数据，或是将前端传来的数据处理得更加适合后端程序，也就是数据映射。再往后交给专业的后端大佬们即可，我们就不管了。

### Electron

`Electron` 作为一个跨平台的 `GUI` 工具，使用 `Chromium` 和 `Node.js` 构建，实现了使用 `JavaScript` 开发桌面应用程序。它也可以算是一个 `JavaScript` 的宿主环境，但其实相当于实现了一个浏览器。`JavaScript` 也被分为两部分，这两部分的宿主环境分别为 `Node.js` 和浏览器。具体可以查看 [Electron 官网](https://electronjs.org/)。

## Event Loop

不管 `JavaScript` 程序运行在 `Node.js` 端，还是运行在浏览器端，都是以单线程的形式运行在宿主环境下。那么，`JavaScript` 是如何处理多任务的呢？

> 异步 + `Event Loop`

简单描述一下：`JavaScript` 会调用宿主环境提供的 `API` 处理不同的任务（这些任务运行在别的线程），并设置这些任务的回调。当这些任务完成时，会在事件队列中放入任务对应的回调，而 `JavaScript` 主线程会不断处理事件队列中的任务，这个过程就被称为 `Event Loop`。

由于这个过程并不在 `ECMAScript` 所规定的规范中，因此，不同宿主环境的实现是有区别的。具体可以查看我之前写的文章：

- [浏览器下的 Event Loop](/wwh-browser-loop)
- [Node.js 下的 Event Loop](/wwh-node-loop)

## 语言特点

每种语言都有自己的特点，`JavaScript` 也不例外，下面就谈谈 `JavaScript` 令人着迷，或是苦恼的地方。

### 数据存储

每种编程语言都需要处理数据和变量的关系，`JavaScript` 也不例外。但作为脚本，它需要运行在一个特定的宿主环境中。这个宿主环境存储了 `JavaScript` 运行时产生的所有数据；又由于闭包的存在，使得这些数据能被各种各样的变量所引用。而 `JavaScript` 中的变量又是无类型的，各种奇奇怪怪的赋值方式会导致各种奇奇怪怪的结果。这可以定义为复杂，但也可以定义为灵活。熟悉它，你可以畅游在 `JavaScript` 的世界中，笑谈程序；而霸王硬上弓，却会陷入无尽的 `bug` 中。

那么，`JavaScript` 中的数据是如何存储的？请查看：[JS 变量存储？栈 & 堆？NONONO！](/wwh-js-memory)

### 作用域 & 闭包

作用域大家都很熟悉，由双大括号产生（`ES6+`）。内层变量可以引用外层变量，而外层变量却对内层变量无能为力。但函数的闭包却打破了这层限制，让外部程序有了改变或获取内部变量的能力。同时，由于数据存储的原因，外部程序在一定情况下甚至可以对内部变量为所欲为。

那么，到底何为作用域？闭包在这里扮演什么角色？请持续关注~

### 函数

`JavaScript` 是一种函数先行的语言，表现在代码上为：可以直接定义 `function` 或是直接将 `function` 赋值给某个变量。

函数，何为函数？函数可以理解为一种行为、一段过程或一个任务，而这些都有一个显著的特点：有起始点和终点。对应到函数上，即为入参和返回值。在面向对象编程被大肆宣传的情况下，函数式编程却在发挥着独特的魅力，以其自然、清晰、简洁的编程方式吸引着众人的目光。

函数式编程是一个很大的话题。什么是函数式编程，光解释是没有用的，只有亲自上手体验一把，才能感受到它的魅力。

有兴趣的朋友可以翻阅 [JS 函数式编程指南](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/)。

### 原型链

前面说到，`JavaScript` 是一种函数先行的语言，那么 `JavaScript` 就不能面向对象了？错！`ES6 class` 语法的出现，标志着 `JavaScript` 完全能够面向对象。查看经过 `Babel` 转码后的 `ES5` 兼容代码，可以清楚地知道，`class` 仅仅只是一个语法糖，不然如何进行转码呢？而面向对象实现的关键，是 `JavaScript` 的另一个特点：原型链（`prototype`）。

那么，何为原型链（`prototype`）？原型又是什么？请查看：[JavaScript 对象 & 原型](/wwh-js-object)

### this

谈到 `this`，相信大部分面向对象编程语言的 `coder` 都很熟悉。它常常出现在对象所属的方法中，`JavaScript` 表现出来的行为和这些语言一致，但其本质却有极大的不同。因为在每一个函数下都有 `this` 的存在，那么 `this` 到底是什么？其所代表的又是什么内容？这和 `JavaScript` 的数据存储有点关系，因此，在弄懂数据存储前，`this` 往往难以预测。

`this` 到底是如何取值的？之后会提到，请持续关注~

### 箭头函数

已经说了函数，为何还要单独写个箭头函数？相信大家对于箭头函数的理解，仅仅停留在简化普通函数上。但箭头函数和用 `function` 声明的函数是有区别的。试想一下，在 `Vue` 文档中是否有这么一句话：`XXX` 只接受 `function`。这个 `function` 不代表箭头函数。那么，箭头函数到底是什么？和普通函数又有什么区别？什么情况下该使用箭头函数呢？

请看之后的文章：什么是箭头函数？

### 其他

`JavaScript` 语言的特点还有很多，`ES6 ~ ES10` 陆陆续续更新了不少内容，包括 `Promise`、`Proxy`、`Iterator`、`Generator`、`async` 等。这些都是语言上的更新，具体使用方式查看文档即可。

[ECMAScript 6 入门](https://es6.ruanyifeng.com/)。

当然，也推荐之前阅读《ECMAScript 6 入门》时写下的笔记：[ecmascript6](/tag/ecmascript6/)。欢迎查阅~~

## 模块化

`JavaScript` 的模块化由 `RequireJS（AMD）` 开启，终结于 `ES6（Module）` 规范，中间经历了 `SeaJS（CMD）` 和 `Node.js（CommonJS）`。目前常用的为 `CommonJS（Node.js）` 和 `Module（ES6 + Babel）`，其他的我们可以略过。

### CommonJS

`CommonJS` 为 `Node.js` 端的模块系统，内置 `3` 个变量：`require`、`exports` 和 `module`。通过 `require` 引入模块，`exports` 对外导出，`module` 保存该模块的信息。如下所示：

```javascript
'use strict';

var x = 5;
function addx (value){
    console.log( x + value );
}

exports.addx = addx;
```

相信大家都知道，导出还有一种写法：

```javascript
module.exports.addx = addx;
// or
module.exports = { addx };
```

其实，只要知道 `module.exports === exports // true`，那么一切就解释得通了。`exports` 仅为 `module.exports` 的一个引用，但是以下写法是有问题的：

```javascript
exports = { addx };
```

出现问题的原因是，`exports` 只是一个引用，如果重新赋值，就会导致该变量引用到另一个数据上。这样一来，结果就是 `module.exports === exports // false`。而模块的信息是以 `module` 为标准的，因此就变成了无导出。如果要究其原因，看看为什么会发生引用变化，可以了解一下 `JavaScript` 中的数据是如何存储的。虽然还没写，但肯定会写的。

### Module

`Module` 为 `ES6` 推出的模块化规范，目的在于统一各种不同的规范，由 `import` 导入、`export` 导出。如下所示：

```javascript
import { stat, exists, readFile } from 'fs';

export const a = '100'; 
// or
const a = '100';
export {
    a
}
```

常见的导入语句有：

导入语句                        | 含义
---:                            | ---
`import { xxx } from 'a'`       | 从 `a` 中导出 `xxx` 的属性或方法，数据由 `a` 的 `export` 导出
`import { xxx as yyy } from 'a'`| 从 `a` 中导出 `xxx` 的属性或方法并重命名为 `yyy`，数据由 `a` 的 `export` 导出
`import xxx from 'a'`           | 从 `a` 中导出默认值，并赋值为 `xxx`，数据由 `a` 的 `export default` 导出

常见的导出语句有：

导出语句                        | 含义
---:                            | ---
`export const a = 1`            | 导出一个常量，名字为 `a` 值为 `1`
`export { a };`                 | 导出名字为 `a` 的属性或方法，其值为作用域中变量 `a` 的值
`export { a : b };`             | 导出名字为 `a` 的属性或方法，其值为作用域中变量 `b` 的值，其上为 `export { a : a }` 的缩写
`export default a;`             | 导出默认值，其值为作用域中变量 `a` 的值
`export default 1;`             | 导出默认值，其值为 `1`

常见的错误导出语句：

1. `export a;`
2. `export default const a = 1;`

以上是常见的错误导出语句，如何避免？只要记住以下规则即可：

1. `export` 导出的值，必须要有名字，`const` 或 `let` 这些声明语句规定了该值的名字，`{a: b}`，也规定了名字。
2. `export default` 仅导出值，因此不需要有特殊的方式规定名字。

## 总结

按照惯例，问几个问题：

1. 什么是解释性脚本语言？
2. `ECMAScript` 与 `JavaScript` 是什么关系？
3. `JavaScript` 都有哪些数据类型？
4. `JavaScript` 的流程控制语句与运算符都有哪些？
5. `JavaScript` 的语言特点是什么？
6. `JavaScript` 模块是如何定义的？

## 参考

- [JavaScript 语言的历史](https://javascript.ruanyifeng.com/introduction/history.html)
- [什么是 JavaScript？](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/What_is_JavaScript)
- [在浏览器中使用 ES6 的模块功能 import 及 export](https://segmentfault.com/a/1190000014342718)
- [JavaScript 标准库](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)
