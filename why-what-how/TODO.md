# Why What or How：选题规划

## 系列主线

`Why What or How` 用问题推动技术解释。后续优先补全现有文章已经留下的线索，再从语言机制扩展到浏览器渲染与工程化：

1. JavaScript 作用域、闭包与 `this`
2. 函数、类、原型与继承
3. Promise、`async/await` 与异步流程
4. CSS 文档流与现代布局
5. DOM 的形成与页面渲染
6. JavaScript 模块系统

每篇只解决一个核心问题。主题过大时应主动拆分，不为了覆盖知识点写成 API 清单。

## 已完成

- [x] 01. [JavaScript 变量与内存](./01-wwh-js-memory.md)
- [x] 02. [JavaScript 类型转换](./02-wwh-js-var-exchange.md)
- [x] 03. [CSS Flex 布局](./03-wwh-css-flex.md)
- [x] 04. [CSS 背景](./04-wwh-css-background.md)
- [x] 05. [什么是前端路由？](./05-wwh-js-router.md)
- [x] 06. [什么是 JavaScript？](./06-wwh-js.md)
- [x] 07. [什么是 CSS？](./07-wwh-css.md)
- [x] 08. [什么是 HTML？](./08-wwh-html.md)
- [x] 09. [如何理解 HTML5？](./09-wwh-html5.md)
- [x] 10. [Node.js 事件循环](./10-wwh-node-loop.md)
- [x] 11. [浏览器事件循环](./11-wwh-browser-loop.md)
- [x] 12. [JavaScript 对象与原型](./12-wwh-js-object.md)

## 待写选题

### [ ] 13.《JavaScript 作用域与闭包》

承接《JavaScript 变量与内存》。核心问题：函数执行结束后，局部变量为什么还能继续存在？

可以讲：

- 词法作用域与动态作用域
- 全局、函数和块级作用域
- 执行上下文与词法环境
- 作用域链和变量查找
- 闭包如何形成
- 被捕获变量的生命周期
- 循环、定时器与闭包
- 闭包的用途和内存风险

重点避免：

- 把闭包简单定义成“函数套函数”
- 把闭包和内存泄漏直接画等号
- 只给出语法示例，不解释变量为什么仍然可达

### [ ] 14.《JavaScript 中的 this 到底指向谁？》

承接《什么是 JavaScript？》。核心问题：`this` 的值由函数写在哪里决定，还是由函数怎样被调用决定？

可以讲：

- 普通函数调用
- 方法调用
- `call`、`apply` 与 `bind`
- 构造调用与 `new`
- 箭头函数的词法 `this`
- 严格模式与非严格模式
- DOM 事件回调中的 `this`
- 类方法中的 `this`

重点避免：

- 使用“谁调用就指向谁”概括所有情况
- 忽略严格模式、箭头函数和绑定函数
- 用背诵优先级替代调用过程分析

### [ ] 15.《函数、类与继承》

承接《JavaScript 对象与原型》。核心问题：`class` 与构造函数、原型链究竟是什么关系？

可以讲：

- 构造函数与 `new`
- 实例原型与构造函数的 `prototype`
- 原型继承
- `class` 与 `extends`
- `super`
- 静态方法和实例方法
- 类字段与私有字段
- 组合和继承的取舍

重点避免：

- 简单把 `class` 称作语法糖后停止解释
- 混淆构造函数的原型与实例的原型
- 为了讲语法忽略属性查找和方法共享机制

### [ ] 16.《Promise 与 async/await》

承接浏览器与 Node.js 事件循环。核心问题：异步代码看起来按顺序书写，执行顺序为什么仍然会改变？

可以讲：

- Promise 的三种状态
- 状态不可逆
- `.then()`、`.catch()` 与 `.finally()`
- Promise 链中的值和错误传递
- thenable
- `async` 函数的返回值
- `await` 如何拆分函数
- 微任务执行时机
- 并行、串行与错误处理

重点避免：

- 把 `await` 解释成阻塞整个线程
- 只讲语法，不结合微任务队列
- 用大量输出顺序题代替机制解释

### [ ] 17.《元素为什么会出现在这里？——文档流与布局》

这是《什么是 CSS？》中留下的选题。核心问题：浏览器依据什么规则决定元素的位置和尺寸？

可以讲：

- 正常文档流
- 块级与行内格式化
- 包含块
- 盒模型
- 外边距折叠
- 浮动与清除浮动
- 定位
- BFC
- Flex 与 Grid 在布局体系中的位置

主题较大，可以拆分为“文档流与 BFC”“定位与包含块”“Grid 布局”三篇。

### [ ] 18.《DOM 是怎么长出来的？》

承接《什么是 HTML？》。核心问题：一段 HTML 文本如何变成 JavaScript 可以访问的对象树？

可以讲：

- 字节、字符与编码
- Tokenization
- Tree construction
- 浏览器的容错解析
- DOM 节点与元素
- 属性和 Property 的区别
- 脚本阻塞解析
- `async` 与 `defer`
- `DOMContentLoaded`

重点避免：

- 把 DOM 简单等同于 HTML 源码
- 混淆 DOM 树、渲染树和页面最终像素

### [ ] 19.《从 HTML 到屏幕上的像素》

承接 DOM 与 CSS。核心问题：DOM 和 CSSOM 准备完成后，页面还要经历什么才能显示？

可以讲：

- CSSOM
- 样式计算
- 渲染树
- Layout
- Paint
- 分层与 Composite
- 重排与重绘
- 布局抖动
- `requestAnimationFrame`
- `transform`、`opacity` 与合成层

重点避免：

- 把所有视觉更新都称为重排
- 把合成层描述成越多越好

### [ ] 20.《JavaScript 模块到底导入了什么？》

承接《什么是 JavaScript？》。核心问题：CommonJS 与 ES Module 的导入值、执行时机和依赖关系有何区别？

可以讲：

- 模块作用域
- CommonJS 的加载和导出
- ES Module 的静态结构
- live binding
- 默认导出与具名导出
- 循环依赖
- 浏览器与 Node.js 的模块解析
- 动态 `import()`
- tree shaking 的前提

重点避免：

- 只罗列 `import`、`export` 语法
- 把 CommonJS 的导出对象与 ES Module 的 live binding 混为一谈

## 备选主题

- CSS Grid 布局
- 浏览器存储：Cookie、Web Storage 与 IndexedDB
- 同源策略与 CORS
- JavaScript 垃圾回收
- Web Component 与 Shadow DOM
- TypeScript 的类型擦除与运行时边界
- 前端性能指标与页面加载

## 写作提醒

- 先找到一个常见误解、反直觉现象或无法解释的例子，再确定标题。
- 文章沿着 `why → what → how` 推进，不从 API 列表开始。
- 先建立模型，再用代码、图表或浏览器结果验证。
- 明确区分语言规范、宿主环境与具体实现。
- 结论要说明边界，不用一句口诀覆盖所有情况。
- 文章末尾保留一组能够检查理解程度的问题。
- 涉及运行时、浏览器支持和标准状态时，发布前重新核对当前资料。
