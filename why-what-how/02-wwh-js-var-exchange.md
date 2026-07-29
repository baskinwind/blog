---
title: JavaScript 类型转换
url: /wwh-js-var-exchange
excerpt: `[] == ![]` 为什么是 `true`？加号什么时候做加法，什么时候突然开始拼字符串？与其背下一串怪异结果，不如顺着 JavaScript 的转换规则一步步算清楚。
---

## 前言

这次的 `why what or how` 主题：`JavaScript` 类型转换。

什么是类型转换，以及为什么要进行类型转换？相信绝大多数熟悉面向对象的开发者都知道：是为了匹配类型！

但这是 `JavaScript` 的世界，可以说原本就没有类型系统，那么为什么 `JavaScript` 的世界也有类型转换？

为了容错！并且 `JavaScript` 世界的类型转换也不是发生在对象上，而是在基础类型！

## 类型！

刚说完 `JavaScript` 没有类型，怎么又开始说类型了？

这里指的是基础类型。相信大家对 `JavaScript` 数据类型已经有了充分了解，这里不再啰嗦，主要有以下 `7` 种：

- 数值：`1`、`1.0`、`1e10`
- 字符串：`"string"`、`"foo"`、`"bar"`
- 布尔值：`true`、`false`
- `null`
- `undefined`
- 对象
- `Symbol`：`ES6` 新出的类型，用于标志唯一值

## 转换场景

`OK`，类型共有 `7` 种，很简单。那么，转换发生的条件是什么？

1. 多个不同类型的数据进行了一定的操作，如比较、数学运算（四则运算与位运算）、逻辑运算（`&&`、`||`）、字符串拼接。
2. 单元运算符：`!`、`+`、`-`、`++`、`--`、`~`（位运算中的取反）。

可以用一个词来概括这些场景：冲突。当冲突发生时，便会发生类型转化，而冲突又可以概括为以下两点：

1. 数据间的冲突。
2. 数据与运算符的冲突。

举几个常见的例子：

```javascript
let num = 1;
let str = 'foo';
let bool = true;
let nu = null;
let nd = undefined;
let obj = {foo: 'bar'};

// 比较：不同数据间的冲突
num < str && bool == nu || nd > obj

// 四则运算：数据的冲突 & 数据与运算符的冲突
num + str - bool * nu / (nd + obj)

// 位运算：数据与运算符的冲突
num ^ str | bool & nu | nd >>> obj

// 拼接字符串：数据与运算符的冲突
`bala${num}bala${str}bala${bool}${nu}bala${nd}${obj}`

// 单元运算：数据与运算符的冲突
!num
+str
-bool
++nu
nu++
--nd
nd--
~obj
```

以上场景都需要进行类型转换。不同类型的数据不能直接进行同一种操作，但这些写法又都符合 `JavaScript` 语法，于是产生了冲突。如何解决冲突呢？这就需要进行类型转换，也要谈到一个策略：

> 在 JavaScript 中，是不太希望发生错误的，换句话说，JavaScript 解析器会尽量满足你的需求。

如何理解这句话？这就需要谈到一个词：偏向性。

## 偏向性

从最简单的拼接字符串来讨论这个问题。思考以下问题：

> 在拼接字符串时，你最希望得到的结果是什么？

这是一个很傻逼的问题，字符串啊！当然必须是字符串啊！通过一个例子来说明：

```javascript
let num = 1;
let string = 'string';
let boolean = false;

`${num}${string}${boolean}`
```

上述代码中，由于（\`\`）操作符返回的一定是字符串，因此 `num` 和 `boolean` 就被 `JavaScript` 解析器转换成了字符类型。

这种根据操作符推测数据应该转换成什么类型的特性，我称之为“偏向性”。所有操作符都有其偏向性，总结如下：

| 操作符                        | 偏向性            |
| ---:                          | :---              |
| \`\`（模板字符串）            | 字符              |
| 四则运算（排除 `+`）          | 数值              |
| 位运算                        | 数值              |
| 逻辑运算（`&&`、`\|\|`、`!`） | 布尔值            |
| `+`                           | 字符 `<` 数值     |
| 比较运算（排除相等与不相等）  | 字符 `<` 数值     |
| `===`、`!==`                  | 引用值            |
| `==`、`!=`                    | 引用值 `<` 数值   |

注：

1. 操作符的偏向性可能不止于一种，如 `+` 运算符，其最终的偏向性由运算符两侧的数据类型所确定。
2. 偏向性内的 `<` 符号表示偏向性的优先级，即若前者不能满足要求则使用后者。

## 确定偏向性

这里仅讨论偏向性不确定的操作符，以及如何确定操作符的偏向性。

### `+` 运算符

1. 若 `+` 号两侧的类型中有字符类型，则其偏向性为字符串。
2. 转换为数值进行比较。

参考以下例子：

```javascript
// 字符串 + 数值
'a' + 1             // 'a1'
1 + 'a'             // '1a'
'10' + 1            // '101'
1 + '10'            // '110'

// 字符串 + 其他
'a' + true          // 'atrue'
'a' + false         // 'afalse'
'a' + null          // 'anull'
'a' + undefined     // 'aundefined'

// 不含字符串
true + null         // 1
false + undefined   // NaN 因为 Number(undefined) 为 NaN，而 NaN 的任何四则运算都为 NaN。
10 + true           // 11

'10' + 1 + 1        // '1013'
1 + '10' + 1        // '1101'
1 + 1 + '10'        // '210'
// 连加操作可以认为是两次 + 的集合，如 '10' + 1 + 1 => ('10' + 1) + 1
```

### 比较运算

比较运算：`>`、`>=`、`<`、`<=` ，这里排除相等判断。

1. 若比较运算两侧都为字符类型，则其偏向性为字符串。
2. 转换为数值进行比较。

可参考以下例子：

```javascript
// 都为字符，则使用字符的字典顺序比较
'b' > 'a'           // true

// 表达式下的注释为推导过程
'a' > 1             // false
// => Number('a') > 1       => NaN > 1      => false
'a' < 1             // false
// => Number('a') < 1       => NaN < 1      => false
// NaN 与任何值比较都为 false，这也证明了 'a' 转换成了数值
'10.1' > 1          // true
// => Number('10.1') > 1    => 10.1 > 1     => true
'10.1a' > 1         // false
// => Number('10.1a') > 1   => NaN > 1      => false
// 这说明 js 解析器确实使用 Number 来转换类型，而不是使用 parseInt 或是 parseFloat

true > 0            // true
// => Number(true) > 0      => 1 > 0        => true
false < 1           // true
// => Number(false) < 1     => 0 < 1        => true
true > false        // true
null < 1            // true
// => Number(null) < 1      => 0 < 1        => true
undefined < 1       // false
undefined > 1       // false
// => Number(undefined) > 1 => NaN > 1      => false

'a' > true          // false
```

注：`+` 运算符只要有一侧是字符类型，其偏向性就是字符类型；而比较运算必须两侧都是字符类型，其偏向性才是字符类型。

纵观上述代码，还可以得出一个结论，只要不是数字型的字符串，与任何非字符串比较都为 `false`，其原因在于 `Number('a')` 为 `NaN`，`NaN` 与任何数值比较都为 `false`。

### 相等判断

相等判断包括 `==`、`!=`。这是重头戏，但内容其实也不难。对于什么是引用值，如果不清楚，可以查看我写的另一篇文章：[JS 变量存储？栈 & 堆？NONONO!](/wwh-js-memory)。变量引用的地址值内数据即为引用值。

其偏向性的判断如下：

1. `null` 与 `undefined` 互相相等，但与其他类型都不等。
2. 如果操作符两侧的数据为同种类型，那么比较两侧数据的引用值。
3. 转换为数值进行比较。

可参考以下例子：

```javascript
// 类型一致，其实不需要验证
'a' == 'b'          // false
1 == 2              // false
true == false       // false

// 对象比较
let a = {};
let b = a;
a == {}             // false    引用值不一致
a == b              // true     引用值一致
// ...

// null 与 undefined
null == undefined   // true
null == 0           // false
undefined == 0      // false
null == false       // false
undefined == false  // false
undefined == 'a'    // false

// 不同类型
'1' == 1            // true
// => Number('1') == 1              => 1 == 1       => true
'a' == 1            // false
// => Number('a') == 1              => NaN == 1     => false
'a' == NaN          // false
// => Number('a') == NaN            => NaN == NaN   => false
// 这个例子可以看出 NaN != NaN 是有实际意义的。
'0' == false        // true
// => Number('0') == Number(false)  => 0 == 0       => true
'0' == null         // true
```

附上基础类型转换规则表，以及转换后的最终值：

| - | undefined | null | Number | String | Boolean | 转换调用的函数 |
| ---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 转换为字符 | "undefined" | "null" | String(xxx) | 无需转换 | "true"/"false" | String |
| 转换为布尔值 | false | false | 0 => false; NaN => false; 其他为 true | '' => false; 其他为 true | 无需转换 | Boolean |
| 转换为数值 | NaN | 0 | 无需转换 | Number(xxx) | true => 1; false => 0 | Number |

## 对象转换

上述内容讨论了 `JavaScript` 中基础类型之间的转换规则，以及如何进行转换。那么现在思考一下：对象是如何与基础值进行操作的？请先思考以下代码的执行结果。

```javascript
let demo1 = {};
`${demo1}`
demo1 + ''
demo1 + 1
!demo1

let demo2 = {
    toString(){
        return 'demo2';
    },
    valueOf(){
        return 2;
    }
}
`${demo2}`
demo2 + ''
demo2 + 1
!demo2

let demo3 = {};
Object.setPrototypeOf(demo3, null);
`${demo3}`
demo3 + ''
demo3 + 1
!demo3
```

请先确保心中大致有个答案，不妨用记事本记下来。接下来公布转换规则：

1. 如果操作符的偏向性为布尔值，那么直接转换为 `true`。
2. 如果操作符仅有字符的偏向性，比如：\`\`，调用对象下的 `toString` 方法，如果没有该方法会报错。
3. 其他情况一律调用 `valueOf` 方法，如果没有该方法会报错。
4. 根据上述过程获得的基础类型数据进行转换，得到结果。

那以上 `12` 个的最终结果确定过程及结果如下：

```
`${demo1}`  => `${demo1.toString()}`    => `${"[object Object]"}`   => "[object Object]"
demo1 + ''  => demo1.valueOf() + ''     => "[object Object]" + ''   => "[object Object]"
demo1 + 1   => demo1.valueOf() + 1      => "[object Object]" + 1    => "[object Object]1"
!demo1      => !true                    => false

`${demo2}`  => `${demo2.toString()}`    => `${"demo2"}`             => "demo2"
demo2 + ''  => demo2.valueOf() + ''     => 2 + ''                   => "2"
demo2 + 1   => demo1.valueOf() + 1      => 2 + 1                    => 3
!demo2      => !true                    => false

`${demo3}`  => `${demo3.toString()}`    => 没有 toString 方法，报错
demo3 + ''  => demo2.valueOf() + ''     => 没有 valueOf 方法，报错
demo3 + 1   => demo1.valueOf() + 1      => 没有 valueOf 方法，报错
!demo3      => !true                    => false
```

因此，对象会先转换成基础类型，再进行后续操作，其关键方法为 `toString`、`valueOf`。至于空对象为什么有 `toString`、`valueOf` 方法，以及设置 `setPrototypeOf(xxx, null)` 后为什么这两个方法就没有了，请查看 [JavaScript 对象 & 原型](/wwh-js-object)。

当我以为得到真理时，一个判断的结果却让我大呼惊讶：

```javascript
'a' > []        // true
```

这个判断返回了 `true`！根据前面所说，字符串与非字符串比较时，返回结果永远是 `false`，这点已经通过验证，不会错。问题就出在这个 `[]` 上：它被转换成了什么？

首先可以确定：`[]` 被转换成了字符。但这又和对象的转换规则相冲突。比较运算的偏向性为数值和字符，为什么调用了 `toString`，而不是 `valueOf` 呢？
为了确定这个问题，我把 `Array` 原型下的 `toString` 和 `valueOf` 稍加了修改：

```javascript
let valueOf = Array.prototype.valueOf;
Array.prototype.valueOf = function(...args){
    console.log('触发 valueOf');
    return valueOf.apply(this, args);
}
let toString = Array.prototype.toString;
Array.prototype.toString = function(...args){
    console.log('触发 toString');
    return toString.apply(this, args);
}
'a' > []
// 触发 valueOf
// 触发 toString
// true
```

没错，我劫持了 `valueOf` 和 `toString` 方法，然后执行了一遍。转换类型时，数组确实先执行了 `valueOf`，而后又执行了 `toString` 方法。那么，为什么转换过程中会同时执行这两个方法呢？会不会和 `valueOf` 的返回值有关？

```javascript
[].valueOf()
// []
[].toString()
// ""
```

`[]` `valueOf` 方法的返回值就是它本身，一个空数组，这显然不是一个基础类型，而 `toString` 返回空字符串，是个基础类型。那这时候我又想到一个问题：如果转换的结果始终得不到基础类型，会发生什么？会报错吗？

```javascript
let demo = {
    toString() {
        return {}
    },
    valueOf() {
        return {}
    }
}
demo > 1
// Uncaught TypeError: Cannot convert object to primitive value
```

果不其然，成功报错了。这也给了我一个启示：为了保证 `JavaScript` 能稳定地运行下去，`toString` 方法必须遵从语义，返回一个字符串。

最后，根据以上内容，将对象用于运算操作时，处理步骤更新如下：

1. 如果操作符的偏向性为布尔值，那么直接转换为 `true`。
2. 如果操作符仅有字符的偏向性，比如 \`\`，则调用 `toString` 方法；没有该方法或该方法未返回基础类型时，则调用 `valueOf`。如果没有 `valueOf` 方法，或 `valueOf` 方法未返回基础类型，就会报错。
3. 其他情况一律调用 `valueOf` 方法；没有该方法或该方法没有返回基础类型时，则调用 `toString`。如果没有 `toString` 方法，或 `toString` 方法没有返回基础类型，就会报错。
4. 根据上述过程获得的基础类型数据进行转换，得到结果。

## 小练习

请判断出以下内容的结果：

```javascript
// == 操作
!'0' == '0'
!''  == 1
''   == 0
!'a' == 0
![]  == []
![]  == 0
[]   == 0
!![] == [1]
!''  == [1]
''   == !'a'
!''  == ''
null == []
null == ![]
null == false
null == true

// 比较操作
'a'  > null
null > 'a'
'1'  > null
'a'  > []
```

## 参考

- [JavaScript 中的相等性判断](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Equality_comparisons_and_sameness)

## 扩展阅读

### 为 falsy 的对象！

在相等判断中：`null` 与 `undefined` 互相相等，但与其他类型都不等。

在对象转换规则中：如果操作符的偏向性为布尔值，那么直接转换为 `true`。

这两条不完全正确。

在浏览器的实现中，有一类对象：`document.all`，它是可以与 `null` 或 `undefined` 相等的，并且这一类对象代表的是 `false`。

```javascript
null == document.all            // true
undefined == document.all       // true
!document.all                   // true
```

但这无关痛痒，仅为了文章的正确性，在这里提一下，知不知道都无所谓，开发时用不到，并且，如果有面试官提了这个问题，就让他谈谈这个的具体用处，评论给我，我也想了解了解，或者你反问：`JavaScript` 中代表 `false` 都有哪些值，如果他忘了 `document.all` 就狠狠的嘲笑一番。

P.S.：`document.all` 已经在 `HTML5` 标准中被移除了，因此这个认知就变得更不重要了。

### 逻辑运算

有较真的网友可能发现，对于逻辑运算符（`&&`、`||`）的描述其实也有不准确之处。比如以下代码：

```javascript
let a = true && 0 && 1;
let b = false || 1;
```

`a` 的值为 `0`，`b` 的值为 `1`，都不是布尔值。这涉及 `JavaScript` 的求值问题，可以理解为：在 `JavaScript` 中，逻辑运算按照优先级进行，并从前往后逐步求值（有可能不会进行到最后一步）。如果需要进行下一步判断，则将当前步骤的数据转为布尔值进行运算；如果不需要进行下一步，则返回当前步骤的值。参考以下例子：

```javascript
0 && 1 && 'a'               // 0
1 && 1 && 'a'               // 'a'
1 && 1 && 0                 // 0
0 || 0 || 'a' || 'b'        // 'a'
0 || 0 && 'a' || 'b'        // 'b' && 操作符的优先级高于 || 操作符
```

如果你能正确理解并得出结果，那应该就没问题了。
