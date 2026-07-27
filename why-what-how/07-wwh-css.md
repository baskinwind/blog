---
title: 什么是 CSS？
url: /wwh-css
---

## 前言

CSS（Cascading Style Sheets，层叠样式表）是一门描述文档呈现方式的样式表语言。HTML 负责内容和结构，CSS 决定布局、颜色、字体、动画以及不同设备上的适配方式。

“层叠”是 CSS 最重要也最容易被误解的概念。它不是 DOM 树的层级，也不等同于样式继承，而是一套在多个声明竞争时选出最终值的规则。

## 一条 CSS 规则

```css
.card {
  color: #1f2937;
  padding: 1rem;
}
```

这条规则由选择器和声明块组成：

- `.card` 是选择器。
- `color` 和 `padding` 是属性。
- `#1f2937` 和 `1rem` 是属性值。

浏览器把选择器匹配到元素，再为每个属性计算最终值。

## 层叠如何工作？

同一元素的同一属性可能同时匹配多个声明：

```css
article {
  color: #374151;
}

.featured {
  color: #7c3aed;
}
```

浏览器并非只比较“谁的权重大”。完整层叠会综合考虑：

1. 相关性：选择器是否匹配。
2. 来源与重要性：用户代理、用户样式、作者样式，以及普通或 `!important` 声明。
3. 层叠层：`@layer` 定义的层级。
4. 选择器优先级。
5. 作用域接近度：使用 `@scope` 时参与比较。
6. 出现顺序：前面的条件相同时，后声明者胜出。

动画和过渡也会在层叠顺序中占据特定位置。把 CSS 冲突简单归结为“算分数”，容易漏掉真正决定结果的因素。

## 选择器优先级

优先级通常写成三部分：

```text
ID - 类/属性/伪类 - 类型/伪元素
```

例如：

```css
#app .card:hover h2 {}
```

它的优先级是 `(1, 2, 1)`：

- 一个 ID：`#app`
- 两个类或伪类：`.card`、`:hover`
- 一个类型选择器：`h2`

通配符 `*`、组合器 `>` 和 `+` 不增加优先级。`:is()`、`:not()` 和 `:has()` 本身不增加一格，它们采用参数中最高的优先级；`:where()` 的优先级始终为零。

```css
:where(.prose h2) {
  margin-block-start: 2em;
}
```

这让基础样式更容易被业务规则覆盖。

`!important` 会改变声明在同一来源中的优先顺序，但不应该作为日常冲突修复工具。更好的做法通常是控制层、组件边界和选择器复杂度。

## 继承与初始值

继承是另一套机制。部分属性在元素没有指定值时，会从父元素继承：

```css
body {
  color: #111827;
  font-family: system-ui, sans-serif;
}
```

文本颜色和字体通常会传给后代，而 `margin`、`padding` 和 `border` 默认不会。

CSS 还提供几个通用关键字：

- `inherit`：使用父元素的计算值。
- `initial`：使用属性规范定义的初始值。
- `unset`：可继承属性按继承处理，否则使用初始值。
- `revert`：回滚到较早来源或层叠来源的结果。
- `revert-layer`：回滚当前层中的声明。

## 盒模型

大多数元素都可以看作一个盒子，由内容、内边距、边框和外边距组成。

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

使用 `border-box` 后，声明的 `width` 和 `height` 会包含 padding 与 border，通常更符合组件布局的直觉。

## 布局系统

现代 CSS 有多套互补布局方式：

- 普通流：文档默认布局。
- Flexbox：适合一维排列和对齐，详见 [CSS Flex 布局](/wwh-css-flex)。
- Grid：适合同时控制行和列。
- 多列布局：适合报刊式分栏。
- 定位：用于相对、绝对、固定和粘性位置。

不要用一种布局模型解决所有问题。选择能直接表达结构关系的方案，通常能减少额外容器和覆盖规则。

## 响应式设计

媒体查询可以根据视口或设备特征应用样式：

```css
.layout {
  display: grid;
  gap: 1rem;
}

@media (min-width: 48rem) {
  .layout {
    grid-template-columns: 16rem 1fr;
  }
}
```

容器查询则根据组件容器尺寸调整内部布局：

```css
.card-wrapper {
  container-type: inline-size;
}

@container (min-width: 30rem) {
  .card {
    grid-template-columns: 10rem 1fr;
  }
}
```

容器查询更适合可复用组件，因为组件不必猜测整个视口有多宽。

## 自定义属性

CSS 自定义属性可以保存并复用值：

```css
:root {
  --color-accent: #7c3aed;
  --space-md: 1rem;
}

.button {
  background: var(--color-accent);
  padding: var(--space-md);
}
```

自定义属性参与层叠和继承，也可以被 JavaScript 动态修改，适合主题和设计令牌。它不是 Sass 变量的简单替代：Sass 变量在构建时求值，CSS 自定义属性在浏览器运行时保留。

## 预处理器、后处理器与 CSS Modules

Sass 等预处理器提供 mixin、函数和模块系统；PostCSS 可以通过插件转换或检查 CSS；构建工具还能自动添加兼容性处理。

现代 CSS 已经支持原生嵌套、自定义属性、颜色函数等许多能力，因此是否引入预处理器应取决于项目需求，而不是历史惯性。

CSS Modules 则解决局部作用域问题。构建工具会把模块中的类名映射为局部标识，降低不同组件之间的命名冲突：

```javascript
import styles from './button.module.css';

button.className = styles.primary;
```

它不会改变 CSS 的层叠本质，但能帮助定义组件边界。

## 总结

- CSS 是样式表语言，不是标记语言。
- 层叠、继承和优先级是三个相关但不同的概念。
- 选择器优先级只是层叠的一部分，来源、层和顺序同样重要。
- Flex、Grid、媒体查询和容器查询分别解决不同的布局问题。
- 工具链应该服务于项目，不必为了使用工具而增加复杂度。

## 参考

- [MDN：CSS 基础](https://developer.mozilla.org/zh-CN/docs/Learn_web_development/Core/Styling_basics)
- [MDN：层叠、优先级与继承](https://developer.mozilla.org/zh-CN/docs/Learn_web_development/Core/Styling_basics/Handling_conflicts)
- [CSS Cascading and Inheritance Level 6](https://www.w3.org/TR/css-cascade-6/)
- [Sass 文档](https://sass-lang.com/documentation/)
