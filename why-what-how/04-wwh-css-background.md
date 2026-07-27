---
title: CSS 背景
url: /wwh-css-background
---

## 前言

作为程序员，技术的落实与巩固是必要的，因此想到写个系列，名为 `why what or how`，每篇文章试图解释清楚一个问题。

这次的 `why what or how` 主题：如何使用背景，或者说如何将背景使用到极致？

背景作为 `CSS` 中的老牌属性，相信大家并不陌生，相关属性也不少。希望大家在看这篇文章时，先放下对背景的固有印象，跟着下面的思路，深入地了解浏览器是如何使用这些属性的。

既然想要深入了解，`cosplay` 成浏览器，体验一把背景绘制，是再好不过的了。

那么我们正式成为浏览器，拿起画笔，画出代码所规定的背景。

## 绘制

假设浏览器就是我们，需要解析以下这段 `CSS`，并为元素绘制背景：

```css
.box1 {
    background-color: #000;
    background-image: url(image.png);
    background-size: 40px 40px;
    background-origin: border-box;
    background-position: 10px 10px;
    background-repeat: repeat;
    background-clip: content-box;
}
```

根据绘画经验，画一幅图片大致需要以下前置步骤：

1. 将画布染上基础色，一般的绘画前都会有这一步。
2. 构思绘画内容，毕竟构思好才能画好嘛。
3. 确定内容的绘制大小。

那么按照绘画的步骤，我们先做好预备工作：

1. 将画布染成黑色，由 `bg-color` 所确定。
2. 确定绘画内容，由 `bg-image` 所确定。
3. 绘制大小为 `40px * 40px`，由 `bg-size` 所确定。

接着，我们需要将构思好的画画在画布上。该如何提笔绘画？这时就需要确定以下几点：

1. 这画的起始点在哪？
2. 是否需要重复绘制，直到画满整张画布？

那么对应到这，是这样的：

1. 将图片放在 `bg-origin` 所规定的基准点处，然后偏移 `(10px, 10px)`，偏移量由 `bg-position` 所确定。
2. 上下左右复制该图片，直到铺满整张画布，由 `bg-repeat` 确定是否重复平铺。

现在我们得到了一张画好了的画布，也就是有了内容的元素背景，那么就结束了吗？

不，还有一个属性没有用到：`bg-clip`。该属性规定元素该如何使用背景。根据设置，元素内容对应的画布区域将保留，其他部分将被弃用。

经过以上 `6` 个步骤，就能成功地为元素画上背景。通过上述步骤，我们已经清楚了具体属性的作用，接下来说说这些属性所代表的具体含义。

## 背景属性

根据上面每个步骤中所使用的 `CSS` 属性，逐一解释。

### background-color

设置画布背景色，也就是给画布染色。支持颜色关键字、`16` 进制色值、`rgb`、`rgba`、`hsl`、`hsla` 等写法。

### background-image

确定需要绘制的内容，支持如下写法：

类型    | 值                | 含义
---:    | :---:             | ---
链接    | `url()`           | 引用图片链接，包括使用资源地址以及 `base64` 形式的图片
渐变    | `xxx-gradient()`  | 使用渐变作为图片
none    | `none`            | 不使用背景图

### background-size

确定绘图区域的大小，支持如下写法：

类型    | 值                    | 含义
---:    | :---:                 | ---
关键字  | `contain`             | 绘图区域大小为图片原比例，并且该区域能正好放在该元素内
关键字  | `cover`               | 绘图区域大小为图片原比例，并且该区域刚好能完全铺满元素
长度    | `<length>` `<length>` | 分别规定绘图区域的长宽
长度    | `<length>`            | 绘图区域长宽一致，为上述情况的缩写
默认    | `auto auto`           | 绘图区域为图片的原始大小

**注：**

1. `<length>` 代表所有合法的长度单位，包括 `px`、`em` 等固定长度以及百分比长度，以及 `auto`，`auto`为图片的原始大小。
2. 该属性设置的是 `bg-image` 图片的大小，并不是背景的大小。

### background-origin

确定绘制区域的基准点，支持如下写法：

类型    | 值                | 含义
---:    | :---:             | ---
关键字  | `border-box`      | 以元素（画布）的 `border` 区域的左上角为基准点
关键字  | `padding-box`     | 以元素（画布）的 `padding` 区域的左上角为基准点
关键字  | `content-box`     | 以元素（画布）的 `content` 区域的左上角为基准点
默认值  | `padding-box`     | ---

### background-position

确定绘制区域距基准点的偏移量，支持如下写法：

类型      | 值                                      | 含义
---:      | :---:                                   | ---
单关键字  | `top`/`bottom`/`left`/`right`/`center`  | 将绘制区域放置在规定处，由于单关键字仅规定了一个方向，上下或左右，另一侧居中
双关键字  | 例：`left top`                          | 关键字类型与单关键字一致，分别规定了水平和垂直方向上的位置
双长度    | `<length> <length>`                     | 分别规定了距离基准点水平和垂直的偏移量
单长度    | `<length>`                              | 仅设置水平平移量，垂直为居中，效果为 `<length> center`
默认值    | `0 0`                                   | 偏移量为 `0`，也就是不偏移

**注：** 如果使用关键字规定绘制区域的位置，则最终的位置与基准点无关，仅与元素（画布）大小有关。

### background-repeat

确定绘制区域是否以及如何重复铺满整个元素（整张画布），支持如下写法：

类型    | 值            | 含义
---:    | :---:         | ---
关键字  | `repeat-x`    | 绘制区域水平重复铺满
关键字  | `repeat-y`    | 绘制区域垂直重复铺满
关键字  | `repeat`      | 绘制区域水平垂直都铺满
关键字  | `no-repeat`   | 绘制区域不重复平铺
关键字  | `space`       | 在图像不缩放不被裁剪的情况下，尽可能的平铺
关键字  | `round`       | 在图像可以缩小不被裁剪的情况下，尽可能的平铺
双关键字 | `---`         | `<x轴设置> <y轴设置>` 使用上述关键字分别规定水平垂直效果
默认值  | `no-repeat`   | ---

**注：** 最后两种关键字的使用场景不多。不被裁剪，是指在 `background-origin` 所指明的盒子中尽可能地放入该元素，根据关键字的不同，效果也不一样：

- `space` 使用该关键字后，背景位置不受 `background-position` 影响，水平、垂直方向都尽可能地使用背景图，剩余空间会均分在背景图之间。
- `round` 使用该关键字后，浏览器计算出元素在水平垂直方向最多能放的图片后，多使用一张背景图，并将背景图整体缩小以适应元素大小，之后根据 `background-position` 放置背景图，然后水平垂直平铺。

这两个关键字理解起来比较复杂，建议写个例子仔细研究。

### background-clip

移除画布上不需要的部分，支持如下写法：

类型    | 值            | 含义
---:    | :---:         | ---
关键字  | `border-box`  | 移除 `border` 区域以外的内容
关键字  | `padding-box` | 移除 `padding` 区域以外的内容
关键字  | `content-box` | 移除 `content` 区域以外的内容
关键字  | `text`        | 移除文字区域以外的内容
默认值  | `border-box`  | ---

### 其他属性

绘制元素的背景仅需要以上 `7` 个属性就能完成。`background` 还有两个属性：

1. 规定画布的显示效果：`background-attachment`。
2. 规定多背景时的背景混合模式：`background-blend-mode`。

#### background-attachment

规定图层相对于什么固定，支持如下写法：

类型    | 值        | 含义
---:    | :---:     | ---
关键字  | `fixed`   | 将画布相对于视口（`viewport`）固定
关键字  | `scroll`  | 将画布相对于元素本身固定
关键字  | `local`   | 将画布相对于 `Max(元素内容, 元素本身)` 固定
默认值  | `scroll`  | ---

在了解这个属性前，我们需要更正一下画布的概念。在此之前，我们把背景图简单地画在了画布上，也就是元素上，这是为了方便大家理解和叙述。

这里明确指出，背景图不画在画布上，而 `bg-color` 属性也只是将元素染色。背景图与画布是独立的，因此这里引入图层的概念：背景图绘制在单独的图层上，图层由 `background-attachment` 决定如何绘制在画布，也就是元素上。当 `background-attachment` 为默认值时，图层与元素一致，其预期完全符合上述叙述，也就是 `scroll` 的效果。那么，再来解释另外两个值的效果。

- `fixed`     图层相对于视口固定，也就是整个 `html` ，`background-origin` 无效，基准点为整个视口的左上角，不能改变，图层大小为视口大小，且不受容器位置影响，也就是始终固定在视口上。
- `local`     图层相对于元素内容固定，图层的大小为 `Max(元素内容, 元素本身)`，也就是说会随着内容的滚动而滚动。
- `scroll`    图层相对于元素固定，图层大小即为元素大小，不跟随内容滚动。

这个属性用起来简单，但理解起来较难，希望大家好好地理解一下。P.S.：熟悉使用 PS 的小伙伴应该很容易就能理解。

具体的效果可以查看 `MDN` 上对该属性的介绍：[background-attachment](https://developer.mozilla.org/en-US/docs/Web/CSS/background-attachment)

#### background-blend-mode

定义多背景时，最终背景的呈现效果。这个在多背景里细说。

### 渐变

在介绍 `background-image` 时，提到过可以使用渐变作为背景图，这里就介绍介绍渐变的几种类型，以及语法。

#### 线性渐变

线性渐变效果如下，需要设置渐变方向以及过渡点，浏览器沿规定的渐变方向渐变颜色。

<div style="height:200px;background-image:linear-gradient(to right, #fff, #000)">
</div>

语法如下：

```
linear-gradient( [ <角度> | to <方向关键字> ]? , <停顿点列表> )
```

解释：

1. `[]` 内为可选内容，用于规定线性渐变的方向，可直接使用角度或方向关键字，默认 `180deg`。
2. 角度：为 `0deg` 时，渐变方向为从元素底部到元素顶部；`10deg` 时，渐变方向顺时针旋转 10 度。
3. 方向关键字：`top/bottom/left/right`，使用单个关键字时（如 `top`），关键字代表渐变方向的终点，使用双关键字时（如 `left top`），关键字的组合代表渐变方向的终点。
4. 停顿点列表：色值与长度单位的组合，如 `#000 100px`，代表距离渐变起点 `100px` 处的颜色为 `#000`。当长度为 `0%` 或 `100%` 时，距离可不用写。

例子：

```css
linear-gradient(to left, #333444, #333 10px, #eee 75%, #333 75%, #fff);
```

#### 径向渐变

径向渐变效果如下，需要设置渐变圆心位置，以及过渡点，浏览器从规定的渐变圆心位置向外过渡到每个过渡点。

<div style="height:200px;background-image:radial-gradient(circle at center, #fff, #000)">
</div>

语法如下：

```
radial-gradient( [ <圆形效果> at <圆心位置> ]? , <停顿点列表> )
```

解释：

1. `[]` 内为可选内容，用于规定正圆还是椭圆，以及圆心位置，默认效果由元素确定，圆心为元素正中。
2. 圆形效果：`circle/ellipse` 分别代表正圆以及椭圆，正圆后可跟一个长度值，代表半径；椭圆后可跟两个长度值，代表水平半径长以及垂直半径长。
3. 圆心位置：由关键字或关键字组合或两个长度值组成。单关键字仅代表了圆心的一个位置，另一位置默认居中。
4. 停顿点列表与线性渐变一致。

一些例子：

```css
radial-gradient(circle, #000, #fff);

radial-gradient(circle at center, #000, #fff);

// 以下 4 条代表的内容一致
radial-gradient(circle at top, #000, #fff);
radial-gradient(circle at top center, #000, #fff);
radial-gradient(circle at 50% center, #000, #fff);
radial-gradient(circle at 50% 50%, #000, #fff);

// 以下 2 条内容一致，椭圆是特殊的圆
radial-gradient(circle 100px at 50% 50%, #000, #fff);
radial-gradient(ellipse 100px 100px at 50% 50%, #000, #fff);
```

#### 锥形渐变

锥形渐变效果如下所示，需要设置变换起始轴，以及过渡点，浏览器由规定的变换起始轴绕一圈进行渐变，最后回到变换起始轴。

**注：** 如果没有看到效果，请到 `chrome` 中打开。

<div style="height:200px; background-image:conic-gradient(#fff, #000);">
</div>

语法如下：

```
conic-gradient( [ from <起始轴角度> at <起始点> ]? , <停顿角度列表> )
```

解释：

1. `[]` 内为可选内容，用于规定起始轴的角度和起始点，起始点为元素正中，`0deg` 为起始点向元素顶部，`10deg` 为顺时针旋转 `10deg`。
2. 起始轴角度：默认 `0deg` 可用合法的角度单位，如 `deg`、`turn`、`rad`。
3. 起始点：由关键字或关键字组合或两个长度值组成，单关键字仅代表了起始点的一个位置，另一位置默认居中。
4. 停顿角度列表：由色值与角度单位组合而成，如 `#000 30deg` 代表距离起始轴顺时针 `30deg` 的方向为 `#000`。

#### 小结

目前浏览器实现的渐变就仅有以上 `3` 种，其中锥形渐变的支持力度堪忧，仅有 `Chrome` 支持，但是使用线性渐变和径向渐变已经能够应付对大多数的日常需求了。

## 多背景

背景相关的属性总共有 `9` 种。除了 `bg-color` 外，其他属性都与多背景有关。如果想要设置多背景，仅需为属性填写多个对应值，并用 `,` 隔开。

```css
background-image:   url(a.png), url(b.png);
background-size:    10px 10px,  20px 20px;
background-origin:  padding-box,border-box;
/* ... */
```

经过上述了解，相信大家也都清楚了：除 `bg-color` 外，其他背景属性其实都是为 `bg-image` 服务的，仔细想想是不是这样？

因此，多背景可以简单地理解为多图层。每个图层如何控制背景图显示，在上文中已经解释清楚。现在思考一个问题：图层如何叠加，层叠顺序又是怎样的？

可以用一句简单的话来概括：先写的图层最后渲染，类似于一个栈；层叠效果由 `background-blend-mode` 所决定。

渲染顺序很好理解，那么 `background-blend-mode` 又是什么？

### background-blend-mode

该属性规定了对应图层该如何叠加到画布上。

该属性的值如下：

```
background-blend-mode: normal | multiply | screen | overlay | darken | lighten | color-dodge | color-burn | hard-light | soft-light | difference | exclusion | hue | saturation | color | luminosity
```

## 小结

`9` 个属性其实也不多，只要结合上述渲染过程，理解起来并不复杂。

对了，还有个复合属性，这里就不说了，不是因为懒，是因为不想说，能不用还是不用吧，别和我说什么传输字节少点什么的，代码是给人看的，写那一堆想看懂都难，还是不说了，当然如果是简单的设置背景，一眼就能看明白的还是建议使用复合属性哈~

惯例，提几个问题：

1. 背景的 `9` 个属性是哪些？
2. 绘制步骤如何？
3. 图层和画布的关系？
4. 是否理解了 `background-attachment`？

## 最后分享几个骚操作

- [CSS 渐变背景](https://leaverou.github.io/css3patterns/)
- [CSS 实现滤镜效果 1](https://bennettfeely.com/image-effects/)
- [CSS 实现滤镜效果 2](https://una.github.io/CSSgram/)
- [CSS 渐变效果](https://uigradients.com/#Ver)
- [CSS 渐变选择 1](https://www.grabient.com/)
- [CSS 渐变选择 2](https://mrmrs.io/gradients/)

## 参考

- [MDN background](https://developer.mozilla.org/en-US/docs/Web/CSS/background)
- [不可思议的颜色混合模式 mix-blend-mode](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651552178&idx=2&sn=644c668cb4cf4bef188d570193bea5b4&chksm=8025ae73b752276573f1479831ad86174a05c5222c83d68429f2c44ee7ddc3ea32f1378bf473&scene=21#wechat_redirect)
- [纯 CSS 图片滤镜项目 CSSgram 简介](https://www.zhangxinxu.com/wordpress/2019/06/css-filters-cssgram/)
