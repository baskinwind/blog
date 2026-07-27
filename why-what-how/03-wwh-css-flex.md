---
title: CSS Flex 布局
url: /wwh-css-flex
---
## 前言

作为程序员，技术的落实与巩固是必要的，因此想到写个系列，名为 `why what or how` 每篇文章试图解释清楚一个问题。

这次的 `why what or how` 主题：如何使用 `Flex` ？

## 是什么？

`Flex` - 一种布局方式

在深入了解前，先来看看浏览器的支持情况吧：

![flex 支持情况](https://pic.breeze.red/wwh-hf-support.png)

一片绿色，那还等什么呢？赶紧掌握呀~

先了解了解布局的发展史，在了解历史的基础上在回头来看看 `Flex` ，以史为镜，可以知兴替嘛~

### 布局发展史

- `Table`

作为 `HTML` 的老牌元素，`Table` 虽然作为表格存在，但是很长一段时间却被用来布局，大致的过程就是将设计稿按照表格切好，然后在表格响应位置放上内容。虽然笔者没有经历过这个阶段，但想想也是很复杂的。之前在学校的时候，学校的官网就是用表格来写的页面，不知道现在改了没有。

- `div + css`

可以说这是目前大多数网站的布局方式，按照 `css` 的内容细分为以下 `3` 种方式

1. 利用 `float` 的特性，配合 `margin` 实现左右布局
2. 利用 `inline-block` 进行布局
3. 利用 `position` 进行元素的定位

但是，和 `Table` 一样，`float` `inline-block` 虽然可以用来布局，但 `float` 的出现却是为了解决图文混排，`inline-block` 是为了解决内联元素修改不了大小的问题，因此这俩虽然可以用来布局，但就像披着羊皮的狼，时不时给你的页面整个 `bug` 。这个时期为了解决这些问题，诞生了很多经典的布局方式，想要实现某种网页布局也需要特殊的文档结构，但这就是对的吗？

- `Flex`

为了解决 `Css` 属性的误用（或者说是规范布局），`HTML5` 制定了一个与布局相关的属性：`Flex`。那么 `Flex` 解决了什么？又有什么好处，且听我慢慢道来。

## 布局

离开了 `Dom` 谈 `Css` 那就是瞎扯，那么假设有这样一个 `DOM` 结构：

![flex 元素呈现](https://pic.breeze.red/wwh-hf-dom.png)

使用 `Flex` 必须要有一个容器，在上面的结构中，黑色线框为容器，浅色内容为容器内容。假设黑色线框内为 `div.parent`，浅色内容为 `div.child`。当我们为容器设置以下样式时：

```css
.parent {
    display: flex;
}
```

神奇的事情发生了，它变成了这样：

![flex 各项定义](https://pic.breeze.red/wwh-hf-flex.png)

先来解释解释图上关键字所代表的内容

1. 主轴，内容排列方向，默认从左向右。
2. 交叉轴，与内容排列方式垂直的方向，当主轴为左右方向时，默认从上到下，当主轴为上下方向时，默认从左到右。
3. 容器宽（高），容器所占页面上的大小。
4. 主（交叉）轴起（终）点，代表主（交叉）轴的起始（终止）点。

记好这张图，我们接着来说说 `Flex` 涉及的属性。还有一点需要记住：当元素设置了 `display: flex` 后，其内的内容元素排列不遵循文档流规则，内容块按照主轴方向排列，排满时，如果可以换行，根据交叉轴方向换行继续排列。

## 语法

由于 `Flex` 涉及容器和内容两级元素，这两级元素分别有单独的属性，用于控制最终的呈现效果。先看看容器属性都有哪些。

### 容器属性

以下属性设置容器元素上。

#### flex-direction

---       | ---
---:      | ---
名        | `flex-direction`
值        | `row` \| `row-reverse` \| `column` \| `column-reverse`
默认值    | `row`
含义      | 规定主轴方向

值              | 含义
---:            | ---
`row`           | 内容块（主轴）从左向右排列
`row-reverse`   | 内容块（主轴）从右向左排列
`column`        | 内容块（主轴）从上向下排列
`column-reverse`| 内容块（主轴）从下到上排列

图示：箭头代表主轴方向。

```css
.parent {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

![不同 direction 的效果](https://pic.breeze.red/wwh-hf-direction.png)

#### flex-wrap

---       | ---
---:      | ---
名        | `flex-wrap`
值        | `nowrap` \| `wrap` \| `wrap-reverse`
默认值    | `nowrap`
含义      | 规定交叉轴的方向

值              | 含义
---:            | ---
`nowrap`        | 不换行，容器为单行布局
`wrap`          | 换行，容器为多行布局，主轴为左右时，交叉轴从上到下，主轴为上下时，交叉轴从左到右
`wrap-reverse`  | 换行，容器为多行布局，交叉轴方向与 `wrap` 相反

当属性设置为 `nowrap` 时，内容块单行排列。

当属性设置为 `wrap/wrap-reverse` 时，容器为多行布局，该属性也可以认为是单行和多行布局的区别属性，记住这个区别，会影响布局。

```css
.parent {
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

图示：

![不同 wrap 的效果](https://pic.breeze.red/wwh-hf-wrap.png)

对于 `wrap-reverse` 的表现可以这么理解：

如果说 `flex-direction` 规定了主轴方向，那么 `flex-wrap` 则规定了交叉轴的方向，当该属性值为 `wrap-reverse`，那么交叉轴方向为从下到上（或从右到左），内容在容器内按照主轴和交叉轴的方向排列，就会产生图示的效果。

#### flex-flow

该属性为 `flex-direction` 与 `flex-wrap` 的简写形式，语法定义如下

```css
.parent {
  // flex-flow: <flex-direction> <flex-wrap>;
  flex-flow: row wrap;
}
```

之前说过，`Flex` 容器内部有属于自己的元素排列方式，该属性就定义了容器内元素的排列方式，`flow` 就有流的含义。

#### justify-content

---       | ---
---:      | ---
名        | `justify-content`
值        | `flex-start` \| `flex-end` \| `center` \| `space-between` \| `space-around` \| `space-evenly`
默认值    | `flex-start`
含义      | 规定内容在主轴上的呈现效果

结合之前的图片，我们来分析分析

![flex 各项定义](https://pic.breeze.red/wwh-hf-flex.png)

解释这些属性的具体含义前，我们需要确定主轴以及交叉轴的方向，经过上面的介绍相信大家也应该了解这图对应的 `flex-flow` 为 `row nowrap`，其他的 `flex-flow` 对应其他的图，这点需要清楚。

值              | 含义
---:            | ---
`flex-start`    | 内容块往主轴起点挤。
`flex-end`      | 内容块往主轴终点挤。
`center`        | 内容块往中间挤，容器主轴两边剩余空间相等。
`space-between` | 内容块两端对齐，内容块主轴方向上间距相等，容器主轴两端无剩余空间。
`space-around`  | 将容器主轴剩余空间均分到每块内容的两侧，内容块间距为容器主轴两端剩余空间的两倍。
`space-evenly`  | 均分主轴空间，内容块间距与容器主轴两端剩余空间相等。

有点绕口，看图就能理解了，箭头上的数字代表该容器内各段空间的比例。

![不同 justify-content 的效果](https://pic.breeze.red/wwh-hf-justify-content.png)

#### align-content

---       | ---
---:      | ---
名        | `align-content`
值        | `flex-start` \| `flex-end` \| `center` \| `space-between` \| `space-around` \| `space-evenly` \| `stretch`
默认值    | `stretch`
含义      | 规定容器每行的呈现效果

之前做 `flex-wrap` 介绍时，提到过，该属性决定了容器是否为多行显示，当内容块多行显示时，`align-content` 决定了容器每行的呈现效果，为了方便起见，由内容块组成的行，定义一个名字：容器行。容器行的高度取决于该行元素的最高内容。

值              | 含义
---:            | ---
`flex-start`    | 容器行往交叉轴起点挤。
`flex-end`      | 容器行往交叉轴终点挤。
`center`        | 容器行往交叉轴中间挤，容器交叉轴两边剩余空间相等。
`space-between` | 容器行两端对齐，容器行交叉轴方向上距离相等，容器交叉轴两端无剩余空间。
`space-around`  | 将容器交叉轴剩余空间均分到每个容器行两侧，容器行间距为容器交叉轴两端剩余空间的两倍。
`space-evenly`  | 均分交叉轴空间，内容块间距与容器交叉轴两端剩余空间相等。
`stretch`       | 将交叉轴剩余空间给均分给容器行，需要注意的是该值将直接改变容器行的高度，而不是改变容器行的呈现位置。

该属性的值与 `justify-content` 的属性很像，仅仅多了一个 `stretch` 。但需要注意的是，该值是默认值。

字面的意思不容易理解，对比图中效果就 `ok` 了，箭头上的数字同样为比例，红色线框为容器中容器行所在的位置。

![不同 align-content 的效果](https://pic.breeze.red/wwh-hf-align-content.png)

介绍该属性时说过，该属性定义容器行的呈现效果，那么该属性在 `flex-wrap` 为 `nowrap` 时，有用吗？

没用！ 但是虽然 `flex-wrap` 为 `nowrap` 不存在多行，但是可以简单的理解为容器行就为容器，也就是 `justify-content` 为 `stretch` 的效果，并且不会被改变，也就是说即使设置了 `justify-content` 为其他值，但容器还是呈现出 `stretch` 的效果。

那么如果 `flex-wrap` 为 `wrap/wrap-reserve`，但内容块仅有一行时，有用吗？完全有用，且符合预期。

因此该属性生效的前提就是 `flex-wrap: wrap/wrap-reserve`。

#### align-items

---       | ---
---:      | ---
名        | `align-items`
值        | `flex-start` \| `flex-end` \| `center` \| `baseline` \| `stretch`
默认值    | `stretch`
含义      | 规定内容在每一个容器行中的呈现效果

值              | 含义
---:            | ---
`flex-start`    | 内容块在该容器行中往主轴开始方向挤。
`flex-end`      | 内容块在该容器行中往主轴结束方向挤。
`center`        | 内容块在该容器行中上下居中。
`baseline`      | 该容器行中所有内容块的基线在同一交叉轴上。
`stretch`       | 内容块在主轴方式上撑满该容器行。如果内容块设置了交叉轴方向上的大小，则效果与 `flex-start` 一致。

在平常的使用中，其实 `align-content` 是很少用到的，但是为什么要在 `align-items` 前说明，是因为 `align-items` 的呈现效果与容器行相关，这里再次着重说一下

以主轴左右方向为例，若主轴方向为上下，将说明中的高度换为宽度即可

1. 如果容器为多行（设置了 `flex-wrap: wrap/wrap-reserve`），容器行的高度由每行中最高元素确定。
2. 如果容器为单行（设置了 `flex-wrap: nowrap`），那么该行高度为 `Math.max(容器高度, 该行中最高的元素的高度)`。不管有没有设置 `align-content`。
3. 容器是否为多行并非由呈现效果决定，仅由 `flex-wrap` 确定，即使设置了换行但内容不够换行也为多行。

图示：因为该属性的表现仅与容器行相关，这里以单行容器作为示例。为了展示出 `baseline\stretch` 的效果，例子用 `line-height` `padding-top` 撑起内容高度。

![不同 align-items 的效果](https://pic.breeze.red/wwh-hf-align-items.png)

1. 图中红线为每块内容基线对其后的基线位置，对齐后由基线上最高的元素顶离容器行的交叉轴起点。
2. 图中最后容器中第二块内容高度超出了容器高度，那么容器行高度就为最高的内容的高度。

### 内容属性

以下内容设置在内容元素上。

#### order

---       | ---
---:      | ---
名        | `order`
值        | `<number>`
默认值    | `0`
含义      | 规定内容块的排列顺序

内容块的排列顺序由该属性决定，`order` 大的内容块优先排列，如果 `order` 一样，文档位置靠前的内容块优先排列。

经过上面的内容相信大家知道了，内容块的排列方式由容器元素的主轴与交叉轴决定，那么排列顺序就由该属性决定了。

图示：元素内的数字为该元素的 `order`

![order 从小到大排列](https://pic.breeze.red/wwh-hf-order.png)

#### flex-basis

---       | ---
---:      | ---
名        | `flex-basis`
值        | `<length>` \| `auto`
默认值    | `auto`
含义      | 设置内容块的基础大小

定义内容块的基础大小，为何是基础大小呢？这和后面介绍的两个属性有关。内容块的基础大小表现如下：以主轴方向左右为例

1. 当该值为 `auto` 或未设置时，内容块的基础宽度由内容块内部决定，由 `margin + border + padding + content` 撑开内容块，表现出的效果和 `inline-block` 一致。
2. 当该值为具体长度时，根据 `box-sizing` 分为两种情况：
    1. `box-sizing` 为 `content-box` 时，相当于设置了内容的 `content` 大小。
    2. `box-sizing` 为 `border-box` 时，相当于固定了内容的 `border + padding + content` 的大小。

这里问个问题：设置了 `flex-basis` 后，内容块的在主轴方向上的大小是否就固定了？

错！！ 要记住这个点，内容块的大小永远由 `margin + border + padding + content` 所决定（也就是盒模型），加了 `flex-basis` 仅仅做了某几个值限定而已，这点需要记好。

#### flex-grow

---       | ---
---:      | ---
名        | `flex-grow`
值        | `<number>`
默认值    | `0`
含义      | 设置内容块的扩大比例

默认值为 `0`，并且当该值小于 `0` 时，相当于 `0`。

当内容块在容器行中排列完并且有剩余空间时，该值规定了剩余空间该如何并入内容块中。具体的过程如下

1. 确定容器剩余空间，假设为 `10px`。
2. 计算出该容器行内所有内容块的 `flex-grow` 总和，假设该行中仅有两内容块，分别为 `2` 和 `3`。
3. 计算出每一份对应的宽度，`10 / (2 + 3) = 2`
4. 第一份内容块的 `content` 大小加 `2 * 2 = 4`
5. 第二份内容块的 `content` 大小加 `3 * 2 = 6`

需要注意的是：增加的大小会加到内容块的 `content` 上，并不会去改变内容块的 `margin border padding`。

#### flex-shrink

---       | ---
---:      | ---
名        | `flex-shrink`
值        | `<number>`
默认值    | `1`
含义      | 设置内容块的缩小比例

默认值为 `0`，并且当该值小于 `0` 时，相当于 `0`。

当内容块在容器行中排列结束并超出容器时，该值规定了元素该如何缩小，让元素尽量不超出容器。

需要注意的是，当容器设置了换行后，由于内容块永远都不会超出容器，因此该值在单行容器中生效。

缩小过程如下：

1. 确定内容块排列后超出的容器空间，假设为 `10px`。
2. 计算出该容器行内所有内容块的 `flex-shrink` 总和，假设该行中仅有两内容块，分别为 `2` 和 `3`。
3. 计算出每一份对应的宽度，`10 / (2 + 3) = 2`。
4. 第一份内容块的 `content` 大小减 `2 * 2 = 4`。
5. 第二份内容块的 `content` 大小减 `3 * 2 = 6`。

需要注意的是，该过程仅是在理想状态下的过程，有可能内容块的大小可能不够减，那么该内容块会尽量把其中的内容显示出来，而内容块依然会超出容器。

`flex-grow` 与 `flex-shrink` 会修改内容块的默认大小，也就是 `flex-basis` 设定的值，也就是说内容块的大小由这 `3` 个值所确定。

#### flex

---       | ---
---:      | ---
名        | `flex`
值        | `none` \| `auto` \| `<length>` \| [ `<flex-grow>` `<flex-shrink>` `<flex-basis>` ]
默认值    | `1`
含义      | 为 `flex-grow` `flex-shrink` `flex-basis` 的复合属性

值              | 含义
---:            | ---
`none`          | 等效于 `0 0 auto`
`auto`          | 等效于 `1 1 auto`
`length`        | 等效于 `1 1 length`

#### align-self

---       | ---                                                               
---:      | ---
名        | `align-self`
值        | `auto` \| `flex-start` \| `flex-end` \| `center` \| `baseline` \| `stretch`
默认值    | `auto`
含义      | 规定该内容块在所在容器行的呈现效果

该值属性和容器的 `align-items` 表达的效果一致，`auto` 为使用 `align-items` 所规定的值，若设置了其他值，则容器所规定的 `align-items` 在该内容块上无效。

图示：在每一个容器的第二个内容块上设置了 `align-self: flex-start`

![不同 align-self 的效果](https://pic.breeze.red/wwh-hf-align-self.png)

ok 到此与 `Flex` 相关的 `12` 属性已解释完毕，由于该属性作为一个布局属性，相信在 `HTML` 中用的还是很多的，因此在说说应用吧。

## 应用

该节为在实际项目中的经验之谈，如有不得当的地方欢迎指出。

在 `scss` 中，我经常会有如下定义

```css
.x-flex {
  display: flex;
  &.x-m-l2r {
    flex-direction: row;
  }
  &.x-m-r2l {
    flex-direction: row-reverse;
  }
  &.x-m-t2b {
    flex-direction: column;
  }
  &.x-m-b2t {
    flex-direction: column-reverse;
  }
}
```

很明显，只要设置了 `x-flex` `class` 就能有一个 `Flex` 容器，`m` 代表主轴，`l2r` 代表主轴方向，从左到右。这样我们就很容易定义一个单行的 `Flex` 容器，方向仅需要在加一个对应的类名即可。

那多行容器呢？仅需在加一个交叉轴的定义即可，代码如下

```css
.x-flex {
  display: flex;
  // ...
  &.x-a-t2b, &.x-a-l2r {
    flex-wrap: wrap;
  }
  &.x-a-b2t, &x-a-r2l {
    flex-wrap: wrap-reverse;
  }
}
```

`a` 代表交叉轴。

经过前面的反复强调，相信大家对于：容器的多列布局由 `flex-wrap` 开启，开启后由 `flex-wrap` 确定容器交叉轴的方向。已经清楚了。

有了上诉两段代码，容器的 `flow`（内容块布局方式）也就可以确定了。

接着我们结合内容块在容器行中的呈现，一般我们都是需要居中呈现的，那么就可以得到以下代码

```css
.x-flex {
  display: flex;
  // ...
  align-items: center;
  justify-content: center;
  &.x-m-start {
    justify-content: flex-start;
  }
  &.x-m-end {
    justify-content: flex-end;
  }
  // ... 类似定义
  &x-a-start {
    align-items: flex-start;
  }
  // ... 类似定义
}
```

其默认值为居中呈现，`x-m-name` 代表主轴方向上的空间分配，`x-a-name` 代表交叉轴方向上的空间分配。

那么容器行呢？同理（首先要确保理解了容器行的概念，没理解可以返回在看一遍）

```css
.x-flex {
  display: flex;
  // ...
  align-content: center;
  &.x-r-start {
    align-content: flex-start;
  }
  &.x-r-end {
    align-content: flex-end;
  }
  // ... 类似定义
}
```

`r` 代表容器行相关定义，`x-r-name` 代表交叉轴上容器行的空间分配。

这样基本上所有容器相关的概念就都转换为语义上的表达了，这里列几个常用容器

布局                        | 内容块左右布局    | 内容块上下布局    | `className`
---:                        | :---:             | :---:             | ---
单行从左到右                | 居中              | 居中              | `x-flex`
单行从右到左                | 居右              | 居上              | `x-flex` `x-m-r2l` `x-m-start` `x-a-start`
多行主轴右左交叉轴下上      | 居右              | 居下              | `x-flex` `x-m-r2l` `x-a-b2t` `x-m-start` `x-a-start`
多行左右下上，容器行居上    | 居右              | 居下              | `x-flex` `x-a-b2t` `x-m-end` `x-a-start`
...                         | ...               | ...               | ...

当然这避免不了在 `class` 上写过多的类名，那我们先发散一下思维，既然 `flex` 是一种结构，那么直接放在 `HTML` 标签上可以吗？

当然可以，这里抛砖引玉一下：

```scss
[flex] {
  display: flex;
  align-items: center;
  justify-content: center;
  align-content: center;
  &[f-m=l2r]{
    flex-direction: row;
  }
  // ...
}
```

那么 `HTML` 就可以这么写

```html
<div class="parent" flex f-m="l2r">
  <div class="child">1</div>
  <div class="child">2</div>
  <div class="child">3</div>
  <div class="child">4</div>
</div>
```

然后将该份 `CSS` 保存在 `CDN` 上，该份文件很小不会被改变，且可以在不影响 `class` 使用的情况下直接定义文档结构，是不是美滋滋啊 ~

## 总结

本文讨论了 `Flex`，包括它的原理，使用技巧等。最后在强调一下，好好理解下容器行的概念，这点在多行布局中至关重要，也只有理解了这个概念，`flex` 使用起来才能得心应手，当然你也可以把 `Flex` 当成是一个用于元素绝对居中的样式属性，但希望在看了这篇文章之后，对 `flex` 有一个新的看法，仅仅用来绝对居中实在是大材小用了。

照例，问几个问题

1. `flex` 容器都有哪些属性？
2. `flex` 内容块都有哪些属性？
3. 那几个属性会导致内容块发生大小变化？有 `4` 个
4. 容器行是什么？

## 参考

- [CSS Flexible Box Layout Module Level 1](https://drafts.csswg.org/css-flexbox-1/#propdef-justify-content)
