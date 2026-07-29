---
title: 什么是 HTML？
url: /wwh-html
---

## 前言

作为程序员，技术的落实与巩固是必要的，因此想到写个系列，名为 `why what or how`，每篇文章解释清楚一个问题。

作为 `why what or how` 的第一章，先选一个较为简单的话题：什么是 `HTML`？

## 释义

`HTML`——`HyperText Markup Language`，超文本标记语言。

> 标记语言，一类以固定的形式描述文档结构或是数据处理细节的语言，一般为纯文本形式，其内容作为其他程序的输入。

因此，`HTML` 是一种用于描述网页的标记语言，纯文本，其内容作为浏览器的输入。浏览器会解析 `HTML` 文本内容，最终呈现页面。

### 那么超文本又是什么？

在网页出现前，人们的阅读习惯是从前往后、从上到下，就像阅读小说，必须从头到尾地看完。思维是单向的，并不存在岔路口。而[超文本][1]从字面上理解，就是超出了一般的阅读习惯：看文章时，思维是发散的，即通过链接的形式跳出当前的思维流，转到另一个页面。故而，这些可以点击跳转的区域，我们称之为“超文本链接”（具体到 `HTML` 标签上就是 `a` 标签）；而具有这种特性的标记语言，理所应当地被称为超文本标记语言。

那么，为何浏览器偏偏要接收 `HTML`，而不接受其他标记语言呢？这就不得不谈谈 `HTML` 与浏览器的诞生。

## 历史

1. `1990` 年，`Tim Berners-Lee` 提出了[超文本][1]的概念。
2. `1991` 年，`Tim Berners-Lee` 在 [SGML][2] 的基础上定义了 `HTML`，并发布了 `WorldWideWeb` 浏览器。为了避免同 `WWW` 混淆，这个浏览器后来改名为 `Nexus`。同年，世界上第一个站点 [https://info.cern.ch/](https://info.cern.ch/) 发布。
3. `1993` 年，`IETF（国际互联网工程任务组）` 正式开始制定 `HTML` 规范。同年，`Mosaic` 浏览器发布，`Web` 的概念正式流行起来。之后的浏览器仍然在沿用 `Mosaic` 的图形化操作界面思想。
4. `1994` 年，`Tim Berners-Lee` 为了推动 `Web` 发展而成立了 `W3C`。同年，`Netscape` 成立，发布了第一款商业浏览器 `Netscape Navigator（Firefox 的前身）`。
5. `1995` 年，`IETF` 发布 `HTML 2.0` 版本。同年，`IE 3.0` 正式发布，浏览器战争爆发。
6. `1996` 年，`W3C` 接管了 `HTML` 的标准化工作。同年，`Opera` 浏览器发布。
7. `1997` 年，`W3C` 发布 `HTML 3.2` 推荐标准。
8. `1998` 年，`Netscape` 在与 `IE` 浏览器的战争中失利，将 `Netscape Navigator` 开源。
9. `1999` 年，`W3C` 发布 `HTML 4.0`。
10. `2000` 年，`HTML 4.0` 成为 `ISO（国际标准化组织）` 标准。此后，`W3C` 致力于 `XHTML`。
11. `2002` 年，`IE` 成为主流浏览器。
12. `2003` 年，`Safari` 携 `WebKit` 渲染引擎登场。
13. `2004` 年，由于 `Web` 的高速发展，`HTML 4.0` 中一些不合理的设计以及缺失的特性开始暴露。由于不满 `W3C` 想要放弃 `HTML`，浏览器厂商发起并组织了 `WHATWG` 工作组，推进 `HTML` 继续发展，也就是 `HTML5`。
14. `2004` 年，`Firefox` 登场，第二场浏览器战争爆发，`IE` 溃败。
15. `2007` 年，`WHATWG` 和 `W3C` 握手言和，一起制定 `HTML5` 标准。
16. `2008` 年，两个工作组发布第一份草案。同年，`Chrome` 携 `V8` 解析引擎登场，加速战争、统一战场。
17. `2014` 年，`HTML5` 发布。同时，`HTML` 不再基于 [SGML][2]，而是作为一门单独的标记语言出现。
18. ...

纵观浏览器与 `HTML` 的发家史，无论是浏览器的起起落落，还是 `HTML` 版本的更新迭代，源头都是 `Tim Berners-Lee`。或许，`HTML` 的命名与浏览器能够解析 `HTML` 只是一种偶然，但这两种事物的出现却是一种必然。而这背后的主导者是互联网，`Tim Berners-Lee` 也仅是执笔而已。为了纪念，他所创建的第一个站点也将被永久保存：[https://info.cern.ch/](https://info.cern.ch/)。

## 语法 or 结构

![HTML 标签结构][9]

这是 `HTML` 中的一个 `p` 标签。在 `HTML` 中，大部分元素都有着同样的结构：

- 开始标签（Opening tag）
- 结束标签（Closing tag）
- 标签内容（Enclosed text content）
- 标签属性（An attribute and its value）——以键值对的形式存在于开始标签上
- 标签类型——开始标签中的第一个单词

只要是符合以上 `5` 个条件，就是一个合法的 `HTML` 标签。

当然，大部分标签都有如上的结构，但还有少部分标签并没有标签内容，因此结束标签也就不是必要的，比如引入图片：

```html
<img src="xxx.jpg" />
```

但是，针对于这种标签，需要在开始标签之后加上 `/` 代表该标记结束。

仅仅定义标签当然是不够的，一个页面的呈现还必须有合理的结构。

而 `HTML` 结构的定义很简单：标签内容是标签的集合。这就像是函数的递归，嵌套的元素就构成了页面的结构。

> 在 `HTML` 中，一段文字、换行等文本内容也是一种元素，属于文本节点。这种节点仅有内容，而不具备标签的结构。

### HTML 默认结构

正所谓没有规矩，不成方圆，想要让浏览器认识并成功显示页面需要有一个统一的模板，如下所示：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>my site</title>
  </head>
  <body>
    <p>hello world</p>
  </body>
</html>
```

从上到下，每一行所代表的含义如下：

1. 声明该文档为 `HTML` 文档，由于 `HTML` 的发展，版本迭代了不少，各个版本的文档声明又不尽相同，而 `HTML5` 独立成一门单独的标记语言后，就不需要像之前版本一样写各种各样的声明，因此就简化为 `<!DOCTYPE html>`。
2. 页面根节点开始，标志着文档解析的开始。
3. 页面头节点开始，其子节点主要是告知浏览器那些需要知道、但不需要呈现在页面上的信息。
4. 通知浏览器使用 `utf-8` 编码集呈现网页。
5. 通知浏览器该页面的标题为 `my site`，通常浏览器会将该内容呈现在标题栏上。
6. 页面头节点结束。
7. 页面内容节点开始，其子节点为整个网页需要显示的内容，浏览器会渲染该内容。
8. 一行段落，内容为 `hello world`。
9. 页面内容节点结束。
10. 页面根节点结束，文档解析结束。

因此，我们可以大致知道，浏览器需要我们提供两部分信息：

1. 一些与页面呈现无关、但需要通知浏览器的信息，即 `head` 的子元素，用于设置浏览器，比如编码方式、视口大小等。
2. 页面的结构，即 `body` 的子元素，用于呈现页面。

除此之外，由于 `head` 中不需要有结构（信息只要一段一段的给即可，不需要嵌套），因此 `head` 中的元素只有一层，即没有子元素。

那么我们可以使用的标签都有哪些？

## head 中的标签

在 `head` 中所能使用到的标签总共只有 `6` 个，如下：

### title

定义文档标题，通知浏览器页面的主体内容，浏览器一般将此信息置于标签栏，同时收藏或查看历史记录时，会以该信息作为关键字。形式如下：

```html
<title>文档标题</title>
```

### base

为网页上所有的超文本链接规定默认地址。浏览器在处理 `body` 中的链接时，会以该标签规定的默认地址为基准。该标签主要规定了两个内容：

- `href`——所有链接都以该属性的值作为默认链接
- `target`——规定浏览器打开某一链接的方式
  - `_blank`——在新窗口中打开
  - `_self`——在当前窗口中打开（如果是 `iframe` 中的文档，则在当前 `iframe` 中打开）
  - `_parent`——在父窗口中打开（常用于 `iframe` 嵌套）
  - `_top`——在顶层窗口中打开（常用于 `iframe` 多层嵌套）
  - `framename`——在指定的 `iframe` 中打开

一个简单的例子：

```html
<base href="https://xxx.com" target="_blank" />
```

### style

样式标签，其标签内容规定了页面的样式信息。可以在 `MDN` 上查看：[CSS 介绍][7]。

### script

脚本标签，其标签内容为浏览器需要执行的脚本信息。可以在 `MDN` 上查看：[JavaScript][8]。

### noscript

当页面不支持 `script` 或是用户主动关闭了 `script` 的执行时，浏览器将该标签内容作为提示信息，用于提示用户。

### link

通知浏览器请求外部资源、丰富网页内容或优化体验，常见用法如下：

- 设置网页的 `icon`

```html
<link rel="icon" type="image/x-icon" href="/path/to/icons/favicon.ico" />
```

如何使用网页 `icon`，以及浏览器具体对这个 `icon` 做了什么，可以查看：[详细介绍 HTML favicon][3]。

- 通知浏览器使用外部样式

```html
<link rel="stylesheet" type="text/css" href="xxx.css" />
```

这种方式相信大家都已经耳熟能详了，主要是让样式分离，方便管理。

- 提供 `rss` 订阅

```html
<link rel="alternate" type="application/rss+xml" title="RSS" href="rss/path" />
```

这个主要是通知搜索引擎当前页面的 `RSS` 订阅在哪里，浏览器上的插件也有能力获取该信息。

- 提供网页主页面

```html
<link rel="canonical" href="main/page/path" />
```

主要用于搜索引擎，告知当前页面是附属于某个页面，引导搜索引擎去加载主页面。

- 优化用户体验

```html
<link rel="dns-prefetch" href="//xxx.com" />
```

通知浏览器提前获取 `xxx.com` 站点的 `DNS` 信息。当页面中有大量关于 `xxx.com` 站点的请求时，添加该信息会加速数据的获取。

```html
<link rel="prefetch" href="source/uri" />
```

通知浏览器提前抓取相应资源的内容，当用户加载该资源时，浏览器就会直接从缓存获取。

```html
<link rel="preconnect" href="//xxx.com" />
```

通知浏览器提前与 `xxx.com` 站点进行 `TCP` 连接，加速资源的获取。

```html
<link rel="prerender" href="//xxx.com" />
```

通知浏览器提前加载 `xxx.com` 站点的所有内容。当用户进入该站点时，页面就会立刻呈现。

以上 `4` 个内容主要用于提升资源的加载速度和用户体验。但利弊是同时存在的：当浏览器加载了大量资源，而用户又没有访问时，就会造成大量资源被消耗却没有得到利用。因此，合理设置才是最正确的。

### meta

最重要的总是最后出场。`meta` 一词的含义就是元数据，该标签主要用于帮助浏览器设置网页信息，常见用法如下：

- 设置网页字符集

```html
<meta charset="utf-8" />
```

从 `HTML5` 开始，使用这种方式声明当前页面所采用的字符集。至于 `HTML5` 以前版本的声明方式，则不推荐使用。

- 设置网页重定向

```html
<meta http-equiv="refresh" content="3;url=https://www.mozilla.org/" />
```

通知浏览器 `3` 秒后重定向到 `url` 所规定的地址。如果地址和当前网页是同一个地址，也就做到了隔 `3` 秒刷新一次的效果。

- 设置网页缩放，以及视口大小

```html
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
```

通知浏览器设置视口，包括宽度、初始缩放比例、最小缩放比例、最大缩放比例，以及是否允许用户进行缩放。

- 告知网页信息

```html
<meta name="author" content="页面作者" />
<meta name="description" content="页面大致描述" />
<meta name="keywords" content="页面关键字" />
```

常用的 `name` 属性就是这些，但还有一些不常用的属性，如 `application-name`、`generator`、`referrer`、`creator`、`robots` 等，可以在 [MDN - META][4] 上了解。

OK，总结一下就两点：

- `head` 中有 `6` 个子元素：`title`、`base`、`style`、`script`、`noscript`、`link`、`meta`。
- `head` 中的元素主要用于告知浏览器一些信息，以及通知浏览器或搜索引擎做出相应动作。

## body 中的标签

`body` 就是描述网页内容的主体了，通过标签的嵌套来描述页面的内容。内容相关的标签主要分为几个大类：

1. 块级标签，用于描述一片区域内的内容。
2. 内联标签，用于修饰一段文本。
3. 列表，用于列出相关联的几项内容。
4. 表格，用于展示二维数据。
5. 图片与多媒体，用于展示图片、视频、音频。
6. 表单，用于用户输入。

至于如何编写 `body` 中的内容，可以参考该篇文章 [文档与网站架构][5]。

网页布局是一门艺术，但是 `body` 中的内容仅仅是标签的嵌套而已。至于如何编写出好看、耐看的页面结构，这里就不过多深入了。

这里列举一些各个分类下的常用标签：

| 类型      | 常用标签                                                                  |
| ---       | ---                                                                       |
| 块级标签  | `p` `h[1-6]` `header` `nav` `main` `article` `aside` `footer` `div` `pre` |
| 内联标签  | `a` `code` `em` `i` `strong` `addr`                                       |
| 列表      | `ul` `ol` `li` `dl` `dt` `dl`                                             |
| 表格      | `table` `tr` `th` `td` `thead` `tbody` `tfoot` `caption`                  |
| 多媒体    | `img` `audio` `video`                                                     |
| 表单      | `form` `input` `textarea` `button`                                        |

这里列举一下常用的通用标签属性

| 属性名    | 含义                                                                      |
| ---       | ---                                                                       |
| id        | 设置 ID，一个元素仅能拥有一个 id，常用于 JS 获取元素                      |
| class     | 设置类名，一个元素可以有多个类名，用于 CSS 设置元素样式                   |
| data-*    | 用于给元素设置一个附加属性，JS 可以获取设置的值                           |
| type      | 用于表单元素，表示该元素为何种表单                                        |


完整的 `HTML` 标签可参照该篇文档：[HTML 元素参考][6]。

## HTML 与 JSP/ASP/PHP

想必大家都访问过 `xxx.com/a.jsp` 之类的网页，那么，是否可以说浏览器请求了一个 `JSP` 文件，并将其解析、展示为网页？

当然不是。能被浏览器解析并最终渲染成页面的，只有 `HTML` 格式的文本。那么，这种类型的链接又该如何解释呢？

在互联网的发展历程中，开发者发现有很多数据需要从数据库或本地文本文件中获取，但是手动编写 `HTML` 文件太过麻烦。以 `Java` 为例，如果网站每天都会发生变化，那么按照生成 `HTML` 文件的思路，只能每天生成一个 `HTML` 文件供用户访问，并且每天都要替换它，这很麻烦。那么该如何解决呢？

这就诞生了 `JSP`。还是上面这个例子，`JSP` 的解决思路如下：

1. 用户请求 `xxx.com/a.jsp`
2. `Java` 程序捕获到用户的请求
3. `Java` 程序寻找相应的 `JSP` 文件
4. `Java` 程序从数据库或本地文件中查出相应信息
5. `Java` 程序将这些信息填充到 `JSP` 文件中
6. `Java` 程序生成 `HTML` 格式的字符流
7. `Java` 程序将该字符流发送给浏览器
8. 浏览器接收到字符流，判断出该字符流为 `HTML` 格式，开始解析并渲染

所以，虽然浏览器请求的后缀为 `JSP`，但最终服务器发送给浏览器的还是 `HTML` 字符流。`ASP/PHP` 等也是类似的流程。因此，`HTML` 是独立于编程语言之外的语言，也可以说 `HTML` 是这些语言的输出，而 `JSP/ASP/PHP` 只不过是辅助程序输出 `HTML` 的中间文件而已。

## 总结

本文叙述了 `HTML` 标准和浏览器的诞生、不断迭代的历史，以及一些 `HTML` 知识。既然以问句开篇，那么就以问句收尾，思考一下这几个问题：

- 何为标记语言？
- `HTML` 标签的结构如何？
- `HTML` 是如何表述整个网页内容的？
- 默认的 `HTML` 文档结构是怎样？
- `head` 的作用是什么？又有哪些子元素？
- 如何提升用户体验？涉及到的标签又是哪个？
- `body` 是做什么用的？常用标签所代表的意义了解了吗？
- 标签的大致分类有哪些？
- `HTML` 与 `JSP/ASP/PHP` 的关系如何？

## 最后

本文仅仅涉及 `HTML`，不涉及 `JS` 和 `CSS`。本文的目的不在于学习 `HTML`，而在于归纳和总结，以及记录自己对 `HTML` 的一些看法，用于在已经了解 `HTML` 的基础上进一步巩固。至于想系统学习的读者，推荐几个网站：

- [Web 入门](https://developer.mozilla.org/zh-CN/docs/Learn/Getting_started_with_the_web)
- [HTML 教程](https://www.w3school.com.cn/html/index.asp)
- [HTML Standard](https://whatwg-cn.github.io/html/)

## 参考

- [MDN HTML](https://developer.mozilla.org/zh-CN/docs/Glossary/HTML)
- [什么是 HTML？](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Getting_started)
- [HTML 的发展史](https://www.cnblogs.com/hynb/p/6014504.html)
- [浏览器近 20 年来的发展简史图](https://software.cnw.com.cn/software-application/htm2009/20091013_183968.shtml)
- [“头”里有什么——HTML 元信息](https://developer.mozilla.org/zh-CN/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata)
- [MDN LINK](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/link)
- [MDN META][4]

[1]: https://zh.wikipedia.org/wiki/Hypertext
[2]: https://developer.mozilla.org/en-US/docs/Glossary/SGML
[3]: https://www.zhangxinxu.com/wordpress/2019/06/html-favicon-size-ico-generator/
[4]: https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta
[5]: https://developer.mozilla.org/zh-CN/docs/learn/HTML/Introduction_to_HTML/%E6%96%87%E4%BB%B6%E5%92%8C%E7%BD%91%E7%AB%99%E7%BB%93%E6%9E%84
[6]: https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element
[7]: https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS
[8]: https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript

[9]: https://blogcdn.acohome.cn/wwh-wh-html-element.png-watermark
