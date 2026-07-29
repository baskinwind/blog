---
title: JavaScript 对象与原型
url: /wwh-js-object
excerpt: 对象从哪里找到自身没有的属性？`prototype`、`__proto__` 和 `constructor` 绕来绕去，究竟是谁指向谁？从一次取值开始，把原型链顺着内存关系拆开。
---

## 前言

这次的 `why what or how` 主题：`JavaScript` 对象 `&` 原型。

此类文章在百度上一搜一大把，其实不用再写了。但是本着把这个问题解释得清清楚楚、明明白白的想法，还是开始写了。

原因如下：

1. 面试宝典类文章，放张图片一糊弄，让人觉得自己理解了。
2. 解释类文章，告诉你一堆语法，对语法一顿解释，告诉你就是这样的，还是没说清楚。
3. 很少有文章单独解释这个点！但这个点是基础！真的很重要！

所以，本篇文章想说一说**对象 & 原型**。但为了确保能顺利理解，请先看完 [JS 变量存储？栈 & 堆？NONONO！][1]。因为该篇文章会从变量存储的角度解释 **JavaScript 对象 & 原型**，请确保看完，再看这篇文章。

## 什么是对象？

既然要说清楚，先问一个最最基本的问题：什么是对象？

对象一句话就能解释清楚：

> 对象是一系列属性 & 数据的集合。

在 `JavaScript` 中创建一个对象的方式有很多，常见的如下：

```javascript
// 字面量直接创建
let obj1 = { foo: 'bar' };

// 通过实例化 Object 类
let obj2 = new Object({ foo: 'bar' });

class A {
    constructor(foo) {
        this.foo = foo;
    }
}

// 通过自定义类创建
let obj3 = new A('bar');
```

以上代码最终都产生了 `{ foo: 'bar' }` 这个对象。对象下有一个叫 `foo` 的属性，它的值为 `bar`。那么，它在堆中是如何存储的呢？

![JavaScript 下对象的存储][2]

看过 [JS 变量存储？栈 & 堆？NONONO！][1]，相信大家对上图应该不陌生。

## 对象的取值

我们继续看一个较为复杂的对象（对象下某个属性也是一个对象）：

```javascript
let complexObj = {
    num: 1,
    str: 'string',
    obj: {
        foo: 'bar',
    }
}
```

在内存中的模型如下：

![复杂对象模型][3]

我们模拟一下 `complexObj.obj.foo` 这个取值过程。

![复杂对象取值过程][4]

取值过程：碰到存储的值是地址值时，就到相应的地址继续进行操作。

总的来说，对象的知识点有两点：

1. 创建对象：在内存堆中开辟一块用于存储一系列 `属性 & 数据` 的空间。
2. 对象取值：根据存储的数据取值，如果是地址值，则到相应内存堆中继续进行操作。

对象到这里就差不多了，那原型又是什么呢？

## 原型 & 原型链

> 原型是对象下的一个属性，每个对象都有，同时原型也是一个对象。

文本描述总是不够直观，我们通过代码来看：

```javascript
let proto = {
    foo: 'bar'
}

let obj = {
    // ...
    __proto__: proto
}
```

设置对象 `__proto__` 属性的过程，就是给对象设置原型，同时该属性指向一个对象。

**注：** 当然 `ES6` 已经不建议这么做了，有专门的方法（`setPrototypeOf`）设置原型，为了解释方便，这里用 `ES5` 的代码作为示例。

有人可能会有疑问：既然是赋值操作，那应该也可以给对象的原型设置为非对象，为什么原型是对象呢？关于这个问题的答案，可以用以下代码测试：

```javascript
let a = {};
a.__proto__ = 1;
console.log(a.__proto__);
```

复制到浏览器即可看到效果。将对象的原型设置为非对象这一操作是被禁止的，也就是说，设置了也没用。

原型的定义也很简单，那么原型是干嘛用的呢？

## 原型的作用

> 原型补充了原对象。当需要访问对象中的属性，但该属性又不存在时，就会去原型上寻找。

按照上面的例子，`obj.foo` 返回什么？以下伪代码就是取值的过程：

```javascript
function getValue(obj, attr) {
    let searchObj = obj
    while (searchObj 下不存在 attr 属性) {
        searchObj = obj 的原型
    }
    return searchObj 下的 attr 属性 
}

getValue(obj, 'foo')
```

以下为图示过程：

![获取原型上的属性][5]

根据上图（或伪代码），我们可以得知最终结果是：`bar`。

那如果对象上的原型还是没有该属性呢？

在原型的定义中提到过：原型同时也是一个对象。转换一下思路，那么这个问题就变成了：如果对象上没有该属性，会发生什么？

去对象下的原型下找！对！我们刚说完！

这种层层递进的关系，我们就把它称为：原型链！

到这你可能会问：如果按照这样一直找下去，那不是无穷无尽了？这就需要引出一个特殊的对象，可以在 `Chrome` 的控制台打印 `Object.prototype` 输出的对象，你可以仔细找找，该对象有没有原型？

`OH NO！` 竟然有原型，你个骗子。

不要激动，继续查看它的原型是什么：`null`！

这和我之前说的：原型也是一个对象，有出入，因为 `null` 明显不是一个对象。

但是，`typeof null` 确实是 `object`，笔者也猜想过是不是和这有关系。但猜想归猜想，为了语义的完整性，我们重新定义一下原型：

> 原型是对象下的一个属性，每个对象都有，同时原型是对象或是 `null`。

同时定义一下原型链的特点：

> 原型链由一个个对象组成，原型链有终点，这个终点是 `null`。

`OK`，既然原型链有了终点，那么，如果一个属性在原对象和所有原型链上都不存在，它的值是什么？

`undefined`（未定义）啊！

现在，我们已经清楚了原型和对象的关系，那么，如何设置对象的原型呢？

## 设置原型

其实上面已经提到过，我们可以直接设置对象的原型：

### ES5 中

```javascript
let proto = {
    foo: 'bar'
}

let obj = {
    // ...
    __proto__: proto
}
```

### ES6 中

由于 `__proto__` 是非标准属性，因此在 `ES6` 中建议使用 `setPrototypeOf` 设置对象的原型。

```javascript
let obj = {};

let proto = {
    foo: 'bar'
}

Object.setPrototypeOf(obj, proto);
```

如果一个对象还没有生成，比如仅仅定义了一个类，但又想控制通过这个类生成的对象的原型，该如何做呢？

既然问了，那肯定是有解决方案的：函数的 `prototype` 属性。

## prototype

我们都知道，在 `JavaScript` 中，函数也是一个对象。这个特殊对象下有一个 `prototype` 属性，它是干什么用的呢？

> 在使用 `new` 关键字调用该函数时，函数下的 `prototype` 属性所保存的对象就是生成对象的原型。

通过代码来解释：

```javascript
function A(bar) {
    this.bar = bar;
}

A.prototype.test = function() {
    console.log('test');
}
A.prototype.testAttr = 'testAttr';

let a = new A('bar');
```

下面用伪代码来解释 `new A('bar')` 这个过程：

```javascript
function fakeNew(A, bar) {
    // 生成 this 为一个空对象。
    let this = {};
    Object.setPrototypeOf(this, A.prototype);
    
    // 执行 A 函数内的代码
    this.foo = bar;
    
    // 将 this 返回
    return this;
}

fakeNew(A, 'bar');
```

相信大家看完代码就能理解 `prototype` 这个属性的作用了。但这是 `ES5` 的代码，`ES6` 中并不建议直接写 `prototype`，而是直接使用 `class`，但其本质是一样的。复制下面的代码到 `Chrome` 里，即可查到真相：

```javascript
class A {
    constructor(bar) {
        this.foo = bar;
    }
    test() {
        console.log('test');
    }
}

console.dir(A);
```

![class 的 prototype][6]

如上图所示，`A` 仍有 `prototype` 属性，并且定义中除了 `constructor` 函数，其他的函数都在 `prototype` 属性内。

可以认为是 `ES5 function` 的语法糖吧，至少这里可以这么认为，但请不要就此认定。相比于 `ES5 function`，它还是有差别的。这部分内容不属于本篇范畴，有机会再单独写一篇讨论讨论。

## 对象的创建

绕了一圈，又绕了回来。现在，我们回过头来看看对象的创建。

通过以上的阐述，我们知道了以下几点：

1. 对象是一系列 `属性 & 数据` 的集合。
2. 原型是对象下的一个属性，它的值是一个对象（或 `null`）。

那么，现在我再问一个问题：用以下代码创建的对象，它的原型是什么？

```javascript
let obj = {};
```

有点疑惑？因为它既没有主动添加原型，也不是从类创建的对象，那它的原型就没有了吗？

答案当然是否定的，只要你把这段代码贴到 `Chrome` 控制台就可以了。想要进一步知道这个对象到底哪儿来的，试试以下代码：

```javascript
obj.__proto__ === Object.prototype; // true
```

很明显，这个新创建对象的原型为 `Object` 这个类的 `prototype` 属性。难不成，这个 `obj` 是通过 `Object` 类创建的？

`bingo~` 你离真相又近了一步。在 `JavaScript` 中，所有对象都由 `Object` 所创建（PS：不管你用什么姿势！）。

好，对象的直接创建弄明白了，再来个间接创建的问题：以下代码所创建的 `obj`，它的原型是什么？

```javascript
function A() {}
let obj = new A();
```

当然是 `A` 的 `prototype` 啊，这还用问？那么，`A` 的 `prototype` 又是什么呢？

放到 `Chrome` 下一看便知：

![function 的 prototype][7]

由上图可见，它是一个简单的对象，最原始的 `prototype` 中仅仅包含了 `constructor` 和 `__proto__`。

`constructor` 就是引用它自己。

```javascript
A.prototype.constructor === A; // true
```

`__proto__` 就是这个对象的原型。只要是一个对象，自然而然会有这个属性，那么，这个属性的值是什么？

```javascript
A.prototype.__proto__ === Object.prototype; // true
```

当然是 `Object.prototype` 啊，所有对象都由 `Object` 这个类所创建嘛！

## 小结

好了，一个简单的点，反反复复，回头一看，竟然这么长了。原本不想写这么长的……看来，原理性的东西虽说不难，但想解释清楚，还是需要一点时间。最后提几个问题，当作小结吧：

1. 对象是什么？
2. 原型又是什么？
3. 对象和原型的关系是什么？
4. 原型和对象的关系又是什么？
5. 原型如何创建，它的默认值是什么？
6. 原型链是怎么组成的？
7. 原型链的终点是什么？

最后，大部分文章提到原型（链）时，都会提到继承。`emmmm`，先放过继承吧。继承只是编程的一种方式，不是原理性的东西，下次讲吧~~

[1]: /wwh-js-memory

[2]: https://pic.breeze.red/post/wwh-wop-object123.jpg
[3]: https://pic.breeze.red/post/wwh-wop-complex-obj.jpg
[4]: https://pic.breeze.red/post/wwh-wop-complex-obj-get-value.jpg
[5]: https://pic.breeze.red/post/wwh-wop-obj-proto-get-value.jpg
[6]: https://pic.breeze.red/post/wwh-wop-class-prototype.jpg
[7]: https://pic.breeze.red/post/wwh-wop-function-prototype.jpg
