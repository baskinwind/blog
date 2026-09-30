---
title: JavaScript 老大的叠盘子游戏
url: /js-call-stack
excerpt: 一只工作盘还没收走，新的盘子又压了上来。递归函数不停喊着“再来一个”，盘子眼看就要顶穿执行大厅的屋顶。门外的定时器急得直跺脚：“我写的可是零毫秒！”
---

![JavaScript 老大端着咖啡，望着即将堆到执行大厅屋顶的工作盘](./cover/13-js-call-stack.png)

## 谁动了我的盘子

“别抽！下面那只还不能拿！”

执行大厅刚开灯，管事的小弟就扑向了工作台。

**JavaScript 老大**端着咖啡，另一只手正捏着最下面的工作牌。上面几张摇摇晃晃，险些全扣进咖啡杯里。

“昨晚就堆在这里，今天还不让收？”

“昨晚留的是示意牌！今天开工前，我把工作牌装进了盘子里，准备演示。盘子代表一次函数调用，下面的还等着上面的交结果呢。”

老大缩回手，想起了[《关不掉的房间》](./12-js-closure.md)里没来得及问完的问题。

“行。今天就看看，这叠盘子到底有什么规矩。”

小弟腾空工作台，打开白幕。

## 先来的先等等

```javascript
function addTax(price) {
    const total = price + 2;
    return total;
}

function makeBill() {
    const price = 10;
    const total = addTax(price);
    return `应付 ${total} 元`;
}

const bill = makeBill();
console.log(bill);
```

“这次从一段普通的浏览器脚本开始。” 小弟先放下标着“脚本”的底盘，“声明函数时先把函数准备好，还没轮到函数体里的代码执行。”

白幕亮到 `makeBill()`。

“叫到我了！” `makeBill` 跑到工作台前。

小弟取出一只新盘子，贴上 `makeBill` 的名字，压在脚本盘上。

“脚本先等着。现在处理最上面这次调用。”

老大执行 `const price = 10`，刚准备继续，便看见下一句：

```javascript
const total = addTax(price);
```

“还得找 `addTax` 算一下。”

又一只盘子落了下来。

```text
栈顶 → addTax       正在计算 price + 2
       makeBill     等 addTax 的结果，再初始化 total
栈底 → 脚本         等 makeBill 的结果，再初始化 bill
```

“我先来的！” `makeBill` 从中间探出头，“怎么又被压住了？”

“你自己叫的帮手。” 小弟指着工作记录，“结果没回来，后面那句怎么做？”

`makeBill` 看了一眼空着的账单，默默缩了回去。

“这叠工作盘就是**调用栈**。” 小弟说道，“一次调用对应的工作记录，通常叫**栈帧**。调用嵌套起来，新的记录放到最上面，这叫入栈。”

“那下面那些函数还在同时干活吗？”

“这里讨论同一条线程上的普通同步调用。当前执行的是栈顶，调用者先暂停，记住回来后该从哪里继续。”

## 一层一层交账

`addTax` 算好了：`10 + 2`，得到 `12`。

“交货！”

执行到 `return total`，结果被送回 `makeBill`。小弟拿走最上面的 `addTax` 盘子。

“这叫出栈。”

`makeBill` 终于重新露出头，把返回值交给自己的 `total`，接着拼出字符串 `"应付 12 元"`，再返回给脚本。

第二只盘子也被收走了。

脚本拿到结果，初始化 `bill`，这才调用 `console.log`，屏幕上出现：

```text
应付 12 元
```

“刚才的示意图没画这些短暂的内置调用，只保留了账单主线。” 小弟补充道。

老大掰着手指算了一遍：“先调用 `makeBill`，再调用 `addTax`；返回时却先结束 `addTax`，再结束 `makeBill`。”

“对，**后进先出**。下面那只等着上面的结果，总不能从中间硬抽。”

“函数没写 `return` 呢？”

“普通函数走到结尾也会返回，返回值是 `undefined`。盘子不会因为少写一行就赖着不走。”

老大把咖啡移远了一点。

“很好，至少这回不用担心打翻。”

## 盘子收了，房间呢

内存清理员听见“收走”，立刻推着小车赶来。

“所有局部变量都装在盘子里？那盘子撤掉，连里面的变量一起倒了？”

“昨天的警报这么快就忘了？” 老大指向隔壁。闭包保存的那把钥匙还挂在门边。

清理员停住了。

小弟翻开[执行上下文的说明书](https://tc39.es/ecma262/multipage/executable-code-and-execution-contexts.html#sec-execution-contexts)。

“盘子只帮助理解调用顺序。规范用**执行上下文**记录代码执行需要的状态，其中会关联用于查找变量的环境。实际引擎的栈帧怎样摆放数据，要看具体实现和优化。”

“所以不能说，每个局部变量都实打实地躺在一个盘子里？”

“不能。更不能说，调用一结束，所有相关数据就立即消失。”

小弟把 `makeBill` 和 `addTax` 的两张工作牌并排放下。

“刚才两次调用里都出现了 `price` 和 `total`，但各自有自己的局部绑定。要解释变量叫什么、到哪里找，看环境；要解释现在执行谁、返回后接着干什么，看调用关系。”

“那昨天的 `createCounter` 呢？”

“那次调用早就返回了，对应的执行记录已经退出调用栈。返回出去的函数仍然引用着外部环境，所以相关状态可以继续存在。用不着把旧盘子一直压在这里。”

清理员把车推了回去。

“明白了，撤工作盘和回收内存，不能当成一件事。”

## 再来一个

门口走来一个函数，名字叫 `sumDown`。

“听说这里能帮忙算账？我想从 `3` 一直加到 `0`。”

“小事。” 老大看向白幕。

```javascript
function sumDown(n) {
    if (n === 0) {
        return 0;
    }

    return n + sumDown(n - 1);
}

console.log(sumDown(3));
```

`sumDown(3)` 的盘子刚摆好，函数就喊了起来：

“我需要 `sumDown(2)` 的结果！”

小弟又放上一只盘子。

“我需要 `sumDown(1)` 的结果！”

再来一只。

“我需要 `sumDown(0)` 的结果！”

老大举起咖啡杯，又放了回去。

“怎么全叫一个名字？自己找自己帮忙？”

“这叫**递归**。” 小弟一边贴标签，一边解释，“函数代码相同，每次调用却有自己的工作记录。这次传入的 `n`，分别是 `3`、`2`、`1`、`0`。”

```text
栈顶 → sumDown(0)   命中终止条件，准备返回 0
       sumDown(1)   等着计算 1 + 返回值
       sumDown(2)   等着计算 2 + 返回值
       sumDown(3)   等着计算 3 + 返回值
```

“写着 `return`，怎么还不走？” 老大指着下面三只盘子。

“得先把 `return` 后面的表达式算完。加法还缺右边的数，怎么交账？”

最上面的 `sumDown(0)` 终于举起手：“我不用再叫了，返回 `0`！”

盘子开始一层层撤下：

```text
sumDown(0) 返回 0
sumDown(1) 算出 1 + 0，返回 1
sumDown(2) 算出 2 + 1，返回 3
sumDown(3) 算出 3 + 3，返回 6
```

屏幕上出现 `6`。

“有来有回，还挺整齐。” 老大终于喝上了一口咖啡。

## 顶到屋顶了

`sumDown` 看着空下来的工作台，忽然来了兴致。

“刚才 `3` 太小，再试个负数呢？”

小弟一把挡住了启动按钮。

“先别运行！这段示例只处理非负整数。传 `-1`，接下来是 `-2`、`-3`，离 `0` 越来越远。”

白幕切到演示模式，工作盘接连出现。

“再来一个！”

“再来一个！”

盘子越堆越高。下面的等上面的，上面的还想找更新的，没有一只交得出结果。

屋顶旁的警报灯亮了。

```text
RangeError: Maximum call stack size exceeded
```

“停！这又不是盖楼！” 老大差点把咖啡泼了出去。

小弟关掉演示，翻出[递归错误说明](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors/Too_much_recursion)。

“这是 Chrome 常见的报错。Firefox 会用 `InternalError: too much recursion`。名字有差异，问题都指向过深的调用。”

“有终止条件也不行？”

“还得能走到。参数要不断接近终止条件，输入范围也得检查。就算最终能到 `0`，普通递归嵌套太深，仍可能先耗尽调用栈空间。”

“那最多能叠多少只？”

“不能给所有浏览器贴一个统一数字。引擎、运行环境和每次调用需要的空间都会影响结果，别把某台电脑测出来的层数当成语言保证。”

老大重新看了一遍算式。

“只是累加的话，不用把未完成的加法全留在下面。拿一个本子，每算一步就记下来。”

```javascript
function sumDownLoop(n) {
    if (!Number.isSafeInteger(n) || n < 0) {
        throw new RangeError("n 必须是非负安全整数");
    }

    let total = 0;
    for (let i = n; i > 0; i -= 1) {
        total += i;
    }
    return total;
}

console.log(sumDownLoop(3)); // 6
```

“循环不会因为每轮累加，再新增一层递归调用。” 小弟收起多余的盘子，“不过这里只是在改调用方式。数字太大时，累加结果仍可能超出安全整数范围；循环次数太多，也照样要花很久。”

“盘子少了，活没少。” 老大叹了一口气。

## 零毫秒也得排队

“咚咚咚！”

定时器回调挤在执行大厅门口，拍得门框直响。

“我早就准备好了，为什么还不叫我？”

老大在白幕上换了一段代码。

```javascript
console.log("开始接单");

setTimeout(function remind() {
    console.log("定时器来催单");
}, 0);

const timedBill = makeBill();
console.log(timedBill);
console.log("当前脚本收工");
```

“这里沿用前面的 `makeBill` 和 `addTax`，在普通浏览器脚本里执行。” 小弟说道。

屏幕依次亮起：

```text
开始接单
应付 12 元
当前脚本收工
定时器来催单
```

“看清楚！我填的是 `0`！” `remind` 把登记表贴到玻璃上。

“`0` 也没有给你一张插队证。” 老大指向还没收完的工作盘，“`setTimeout` 登记完就返回，等待和后续调度由浏览器安排。你的函数体不会在登记时直接执行。”

小弟递来[定时器的工作制度](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#timers)。

“延迟时间到了，相关任务也得等到可以被调度。当前脚本的同步调用没执行完，定时器回调不能跳进来打断。零毫秒不保证立刻执行，更不保证精确的执行时刻。”

“那把累加函数拆成十个小函数，调用十次，总能让门开一会儿吧？”

“如果还是在同一个任务里连续同步调用，就只是换了十只工作盘。中间某个函数返回，不等于把控制权交回事件循环。”

定时器回调垂下了登记表。

“所以盘子堆得不高，也可能一直进不去。”

“对。主线程长时间执行同步代码，会拖延需要主线程的输入处理和页面更新。要让出执行机会，得真正把工作拆到后续任务，或者把适合的计算移到 Worker 等其他执行环境。”

老大刚准备宣布散会，小弟又补了一句：

“也别反过来理解成整个浏览器只有这一叠盘子。浏览器还有其他线程和进程，Worker 也有自己的执行环境。这里盯着的是当前页面主线程上的这条同步调用链。”

## 给盘子拍张照

“以后盘子又卡住，难道都要搬梯子看？” 老大揉了揉脖子。

“不用，调试器能看。”

小弟在最初 `addTax` 的 `return total` 那一行设了断点，重新运行账单示例。

Chrome DevTools 的 `Sources` 面板停了下来，`Call Stack` 中列出当前暂停的调用链，顶部是 `addTax`，往下能找到 `makeBill` 和发起调用的脚本位置。

“点某个栈帧，可以查看对应代码位置和作用域里的值。名字、行号以及是否隐藏部分帧，会受运行方式和工具设置影响。” 小弟把[调试器说明](https://developer.chrome.com/docs/devtools/javascript/reference#call-stack)放在旁边。

“这下知道该找谁催账了。”

“另外，工具有时会补充异步调用的来源记录。那些历史线索，不代表早已结束的函数还压在当前同步调用栈里。”

老大在工作台上贴了三张便签：

- 普通同步调用一层层进入，正常返回时一层层退出；异常也可能沿调用链向外传播。
- 调用栈记录执行关系，词法环境负责变量绑定；栈帧退出不等于相关内存立刻回收。
- 递归既要有可达的终止条件，也要注意深度；减少栈深度，不会自动消除长时间计算造成的阻塞。

“今天盘子的问题，算是说明白了。”

## 旁边还有一扇门

工作台终于清空。定时器回调捏着号码牌，准备跟着下一个任务进场。

隔壁突然传来一声招呼：

“微任务检查点到了，这边还有活！”

一个带着 `Promise` 标记的回调，从事件大厅另一侧探出头。

定时器回调愣住了。

“我等了半天，怎么旁边还有队？”

**JavaScript 老大**看了看咖啡杯，已经空了。

“好吧。明天得把事件大厅的排队规矩也翻出来了。”

## 工作台上的说明书

- [ECMAScript：执行上下文](https://tc39.es/ecma262/multipage/executable-code-and-execution-contexts.html#sec-execution-contexts)
- [ECMAScript：普通函数调用的准备过程](https://tc39.es/ecma262/multipage/ordinary-and-exotic-objects-behaviours.html#sec-prepareforordinarycall)
- [HTML Standard：事件循环处理模型](https://html.spec.whatwg.org/multipage/webappapis.html#event-loop-processing-model)
- [HTML Standard：定时器](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#timers)
- [MDN：递归过深错误](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors/Too_much_recursion)
- [Chrome DevTools：查看调用栈](https://developer.chrome.com/docs/devtools/javascript/reference#call-stack)
