---
title: 服务器不在，我替它回
url: /service-worker
excerpt: 断网警报刚响，Service Worker 就从路边岗亭冲出来，拦下 URL 的请求车：“服务器不在，我替它回。”旧版还没交班，新版又带着一仓库新文件赶到。到底谁有资格站上这个路口？
---

## 谁在半路拦车？

本文承接上一篇：[《主线程今天不接客》](/web-worker)。

断网警报响起时，URL 的请求车已经冲进隧道。

HTTP 抱着急救箱坐在副驾驶，嘴里还叼着半块饼干。

“目标，服务器。任务，取回首页。”

“前面没网了！”URL 猛踩刹车。

车轮擦出一串火星。

隧道尽头的信号灯全部熄灭。TCP 车库拉下卷帘门，服务器方向只剩一片黑。

HTTP 把饼干咽了下去：“掉头。连接都建不起来，还取什么首页？”

URL 刚要挂倒挡，路边一间小岗亭突然亮起橙色的灯。

门被踹开。

一个穿长风衣的年轻人冲到路中央，左手举停车牌，右手拎着一只纸箱。

“停车！这份请求由我处理。”

HTTP 从车窗里探出头：“你谁？”

“Service Worker。”

“服务器的人？”

“浏览器的人。”

“那你凭什么替服务器回话？”

Service Worker 从纸箱里抽出一份首页响应，拍在挡风玻璃上。

“因为上次有网时，我留了一份。”

URL 看了看黑掉的隧道，又看了看响应：“这也行？”

“不但行，还不用掉头。”

HTTP 一把按住响应：“等等。请求还没出浏览器，怎么先有结果了？你这叫半路拦车。”

“说得不够专业。”Service Worker 整了整风衣，“请叫我网络代理。”

“无证拦车也能叫代理？”

两人正要隔着车窗动手，浏览器老大赶到，一脚踩住掉在地上的停车牌。

“都松手。先查证件。”

## 亲戚也不能串岗

Web Worker 也从计算分部跑来看热闹。

“亲戚！”

Service Worker 瞥了一眼那顶毛线帽：“上班第一天就想闯 DOM 工地的那个？”

“家丑不必跨部门传播。”

浏览器老大把两张工作牌并排放在车头。

Web Worker 的牌上写着：

```text
接收页面消息 -> 后台计算 -> 发回结果
```

Service Worker 的牌上写着：

```text
等待事件 -> 处理请求 -> 决定响应从哪里来
```

HTTP 用指关节敲敲第二张牌：“所以它不负责算五百套折扣？”

“能算，不代表该这么用。”浏览器老大说，“Web Worker 通常由页面创建，替页面承担持续计算。Service Worker 则要先由页面向浏览器注册，主要在页面和网络之间处理 `fetch` 等事件。”

Web Worker 往岗亭里看了一眼：“连张床都没有。晚上住哪？”

“不住。”Service Worker 说，“有事件时浏览器把我叫醒，事情办完，什么时候让我休息也由浏览器决定。”

“那你存在柜子里的全局变量——”

“可能下次醒来就没了。”

“咖啡呢？”

“喝快点。”

浏览器老大敲了敲岗亭：“Service Worker 不是常驻后台进程。需要长期保存的数据，放 `Cache Storage` 或 `IndexedDB`，别指望靠内存里的全局变量熬过下一次唤醒。”

HTML 解析器抱着一棵 DOM 小树赶来：“这位能帮我改页面吗？”

两名 Worker 同时往后退了一步。

“不能。”

“终于有一条家规记得很牢。”

## 证件呢？

HTTP 仍然不肯放开那份响应。

“工作内容写得挺漂亮。谁批准的？”

浏览器老大从风衣内侧抽出一份入职档案：“证件在这里。三天前，这家商店第一次申请网络岗亭。”

浏览器老大按下回放键，隧道里的断网警报迅速倒退。画面停在一个网络正常的下午。

JavaScript 老大正对着登记台提交申请：

```javascript
if ("serviceWorker" in navigator) {
    navigator.serviceWorker.register("/sw.js", {
        scope: "/"
    });
}
```

登记员先看页面地址，又看脚本地址，直接把第一份申请推了回来。

```text
页面：http://shop.example.com
结果：拒绝
```

JavaScript 老大拍桌子：“文件、路径、括号，一个没错！”

登记员指了指墙上的警示牌：“能拦请求，也能改响应，权限这么大，还想在明文 HTTP 上入职？”

浏览器老大在申请表上盖下红章：

```text
仅限安全上下文
```

页面换成 `HTTPS` 后，申请终于通过。

“本地开发时，`localhost` 等可信的本地地址通常也能用。”登记员补了一句，“正式上路，走安全连接。”

登记员又把页面地址和 `/sw.js` 并排盖章：“注册脚本还得和页面同源。网络路口的总岗亭，不能从隔壁网站临时雇一个人来守。”

JavaScript 老大又把脚本挪进 `/workers/sw.js`，却仍然申请管理整个网站。

```javascript
navigator.serviceWorker.register("/workers/sw.js", {
    scope: "/"
});
```

登记员看完，第二次把表退了回来。

“这回又怎么了？”

“岗亭修在 `/workers/`，默认最多管这条路径下面的页面。想管根目录，要么把脚本放到更高的位置，要么由服务器通过 `Service-Worker-Allowed` 明确扩大范围。”

Service Worker 在地上展开一张地图，用粉笔圈出自己的辖区。

“`scope` 决定哪些页面可以成为受控页面。圈外页面的车，不能因为我嗓门大就拦。”

URL 举手：“那受控页面请求一张跨源图片呢？”

“也可能经过我的 `fetch` 岗亭。”Service Worker 说，“辖区看的是发起请求的客户端页面，不是只看请求目的地。但别打歪主意，我不能借此拆掉 CORS，也不能凭空获得跨源响应的读取权限。”

浏览器海关在远处抬起头：“谁要拆我？”

“没人。”四个人异口同声。

## 装好了还不让上？

注册通过，Service Worker 拎包走进安装室。

门外摆着一排准备离线使用的资源：

```javascript
const STATIC_CACHE = "shop-static-v1";

self.addEventListener("install", (event) => {
    event.waitUntil(
        caches.open(STATIC_CACHE).then((cache) => {
            return cache.addAll([
                "/",
                "/offline.html",
                "/assets/app.8d2c.js",
                "/assets/app.19aa.css"
            ]);
        })
    );
});
```

安装员数了数：“首页、离线页、脚本、样式。四件。”

Service Worker 已经把一只脚迈出安装室：“那我上岗了。”

安装员拽住风衣后摆：“回来。`waitUntil()` 里的工作还没完成。”

“开个仓库而已。”

话音刚落，搬运工抬来一张 `404` 回执。

```text
/assets/app.19aa.css：未找到
```

整批预缓存当场失败，安装室上方亮起红灯。

Service Worker 的工作牌被收了回去。

“少一张样式而已，让我先拦点别的。”

“不行。”安装员说，“传给 `install` 事件 `waitUntil()` 的 Promise 被拒绝，这一版安装失败。否则你承诺离线可用，仓库里却缺关键零件。”

JavaScript 老大修正文件名，再次提交。四份资源全部入库，红灯终于变绿。

Service Worker 冲向岗亭，又被浏览器老大拦住。

“还来？”

“安装成功，接下来是激活。”

“激活完总能接管刚才那个页面了吧？”

“默认也不能。”

Service Worker 沉默了三秒。

“这份工作最主要的内容，是等待吗？”

浏览器老大把刚打开页面时的事故记录扔给他：“这个页面导航进来时，还没有 Service Worker。为保证一次页面加载使用一致的控制方式，通常要等下一次导航，页面才会受你控制。”

JavaScript 老大按下刷新。

这一次，页面和后续资源请求都经过了岗亭。

“也可以在激活后调用 `clients.claim()`，主动接管辖区内尚未受控的已打开页面。”浏览器老大说，“但页面前半段没经过你，后半段突然换规则，未必安全。别把它当成无脑粘贴的入职口号。”

Service Worker 把这句话写在岗亭门上：

```text
能抢，不等于该抢
```

回放画面暗了下去。

浏览器老大合上入职档案，路口的断网警报仍在响。

HTTP 又检查了一遍响应，终于把手松开：“证件是真的。这次放行。”

URL 抱着缓存首页冲回页面。服务器方向依旧一片漆黑，商店的灯却重新亮了起来。

几分钟后，隧道里的信号也恢复了。

HTTP 理了理白大褂：“离线这次算你赢。接下来查查有网时，你到底怎么拦车。”

## 先开仓，还是先上路？

网络恢复后的第一辆车，是一份脚本请求。

岗亭里的 `fetch` 警铃响起。

Service Worker 抬起栏杆，又立刻放下。

```javascript
self.addEventListener("fetch", (event) => {
    if (event.request.method !== "GET") {
        return;
    }

    if (event.request.mode === "navigate") {
        event.respondWith(networkFirst(event.request));
        return;
    }

    const url = new URL(event.request.url);

    if (
        url.origin === self.location.origin &&
        url.pathname.startsWith("/assets/")
    ) {
        event.respondWith(cacheFirst(event.request));
    }
});
```

HTTP 把车门拍得砰砰响：“又拦！这次网络明明是好的。”

“网络好，也不代表每件货都必须重新跑一趟。”

Service Worker 打开两条通道。

左边写着：

```text
cache first
```

右边写着：

```text
network first
```

带内容哈希的脚本和样式被送进左边。

Service Worker 先查仓库。命中就直接交货；没有，再放行去网络，回来后留一份副本。

```javascript
async function cacheFirst(request) {
    const cache = await caches.open(STATIC_CACHE);
    const cached = await cache.match(request);

    if (cached) {
        return cached;
    }

    const response = await fetch(request);

    if (response.ok) {
        await cache.put(request, response.clone());
    }

    return response;
}
```

HTTP 盯着 `clone()`：“为什么复印一份？”

“响应体是流。原件要交给页面消费，入库需要另一份。”

“你这个仓库会自动听 `Cache-Control` 的安排吗？”

“别把 Cache Storage 和 HTTP 缓存混在一起。”Service Worker 把仓库钥匙转了一圈，“这里存什么、何时读取、何时更新，由代码明确决定。不会因为名字里都有 Cache，就自动替开发者选好策略。”

页面导航则被送进右边。

Service Worker 先尝试网络，拿到新页面便更新备份；网络失败，才去找旧页面或离线提示。

```javascript
const PAGE_CACHE = "shop-pages-v1";

async function networkFirst(request) {
    const cache = await caches.open(PAGE_CACHE);

    try {
        const response = await fetch(request);

        if (response.ok) {
            await cache.put(request, response.clone());
        }

        return response;
    } catch {
        return (
            (await cache.match(request)) ||
            (await caches.match("/offline.html"))
        );
    }
}
```

URL 左看右看：“为什么不全部优先走缓存？快啊。”

“为什么不全部优先走网络？新啊。”HTTP 立刻反驳。

“所以你们两个不适合当调度员。”Service Worker 把两人推出岗亭，“先看运的是什么，再决定先开仓库还是先去服务器。”

争论刚结束，屏幕外又滑进来一块新路牌：

```text
/api/products -> cache first
```

Service Worker 抬头：“谁加的？”

“商品接口也走缓存，打开最快。”开发者的声音从屏幕外传来。

HTTP 敲了敲路牌：“价格变了呢？”

“鞋价又不会天天变。”

Service Worker 犹豫了一下，还是把路牌挂到了左边通道。

## 699 怎么还在？

三天后，运营把一双鞋从 699 元降到 399 元。

主人打开页面，鞋子仍然卖 699 元。

投诉单像雪片一样飞进浏览器总部。

运营跟在投诉单后面冲进来，把后台价格表拍在 Service Worker 桌上：“服务器上明明是 399，页面为什么还敢卖 699？”

HTTP 立刻翻开运输记录。

没有超时，没有报错，甚至没有这次商品接口的出车记录。

HTTP 把空白记录转向 Service Worker：“这辆请求车根本没出过隧道。你在路口把它截了？”

Service Worker 翻开规则簿：“商品接口，cache first。命中旧响应，按章办事。”

旧商品响应从仓库探出头：“三天没人来问过我是不是过期。我以为自己表现稳定。”

HTTP 气得白大褂都鼓了起来：“缓存不是越多越好。你把错误策略执行得这么快，事故来得也很快。”

Service Worker 不服：“我只提供决定权，不替人决定业务能忍受多旧的数据。”

HTTP 一把扯下 `/api/products` 的路牌：“带内容哈希的脚本换了内容，地址也会跟着换。鞋子从 699 降到 399，请求地址可没改。旧响应准备靠闻味道发现降价？”

Service Worker 没接话，默默把商品接口移到 network first 通道，又在离线数据旁挂上“内容可能不是最新”的提示。

开发者在屏幕外小声抗议：“可 cache first 快。”

“错得也快。”HTTP 说。

旧商品响应被移出常用仓库时，还在小声抗议：

“699 也不是不能卖……”

HTTP 抬脚把门关上。

## 旧版凭什么不走？

几天后，`sw.js` 有了变化。

浏览器发现脚本内容不同，带回一名新版 Service Worker。

新版穿着更长的风衣，身后跟着一支搬运队。新的脚本、样式和离线页整整齐齐进入 `shop-static-v2`。

安装员盖章：“安装成功。”

新版昂首走向网络岗亭。

门没开。

里面坐着旧版。

“劳驾，交班。”新版敲了敲窗。

旧版捧着咖啡：“没到点。”

“新缓存已经装好。”

“两个购物页面还归我管。”

“你的脚本都旧了。”

“它们的页面也是按旧版加载的。”

新版转身找浏览器老大投诉：“它都闲得在杯底画地图了，凭什么占着 active 岗位？”

浏览器老大调出登记表：

```text
旧版：active
新版：installed, waiting
受控页面：2
```

“Service Worker 空闲时，执行环境可以被停掉，但旧版和这些页面的控制关系还在。需要处理事件时，浏览器仍会按旧版规则把它叫醒。”

新版指着两张页面登记卡：“让它们刷新。”

第一张页面刷新了。

旧页面尚未完全离场，新导航已经来取响应，两张登记卡在交接窗口撞到一起。

第二张页面从角落探出头：“我还开着呢。”

新版的脸色越来越难看。

“正常情况下，要等旧版不再控制任何客户端，我才能激活？”

“对。关掉相关标签页、让页面导航离开，等旧控制关系真正结束。”

旧版冲新版举了举咖啡：“不是我不想走，是它们不肯关。”

页面立刻反驳：“购物车填了一半，凭什么赶人？”

新版一拍等候室的桌子：“那我就在这里看你们买到明年？”

## 抢班抢出事故了

新版从风衣里抽出一张加急命令：

```javascript
self.skipWaiting();
```

旧版的咖啡还没放下，新版已经越过等候室，强行进入激活流程。

“看见没有？”新版接过岗亭钥匙，“等待不是不能跳过。”

旧版被挤出门外：“年轻人，先看页面版本。”

新版没听见，转身清理旧仓库。

`shop-static-v1` 被整间推走，只留下 `shop-static-v2`。

恰在这时，一张仍按旧版脚本运行的购物页面点下“结算”。

页面伸手索要延迟加载的文件：

```text
/assets/checkout.v1.js
```

新版翻遍 v2 仓库。

没有。

隧道偏偏又断了网。

结算按钮当场躺平。

页面冲进岗亭：“我的文件呢？”

“新文件叫 `checkout.v2.js`。”

“可我是 v1 页面！”

“现在归 v2 管。”

“谁同意的？”

新版默默把 `skipWaiting()` 藏到身后。

浏览器老大没有解释，先把被推走的 v1 仓库拖了回来。

“重新演一遍。页面，你刚才变过吗？”

“没有。我从打开到现在，一直是 v1。”

“岗亭呢？”

新版低声说：“中途换成了 v2。”

“仓库呢？”

“只剩 v2。”

浏览器老大指了指躺平的结算按钮：“三套东西，两个版本。现在知道旧版为什么不肯立刻让位了？”

新版看着一地碎片：“它不是舍不得工作牌，是在等旧页面把这一班走完。”

旧版在门外咳了一声：“这句话请刻在岗亭正面。”

“那我永远不能跳过等待？”

“能。”浏览器老大把 `skipWaiting()` 从新版身后抽出来，“先回答一个问题：旧页面突然归你管，会不会出事？”

新版看了一眼结算按钮。

结算按钮把自己往墙角挪了挪。

“这次会。”

## 页面还没点头呢

第二次交班，新版老老实实坐进等候室。

门外的购物页面收到一张通知：

```text
新版本已准备好，是否刷新更新？
```

页面看了看填到一半的地址：“不更。”

新版隔着玻璃跳起来：“我连新离线页都熨平了！”

“地址填完再说。”

“只要一下！”

“上一个说‘只要一下’的，把我的结算按钮弄死了。”

新版坐了回去。

主人保存好购物车，终于点下“更新”。JavaScript 老大这才把交班通知送进等候室：

```javascript
updateButton.addEventListener("click", () => {
    registration.waiting?.postMessage("PLEASE_ACTIVATE");
});
```

新版拆开通知，取出那张一直想用的命令。

```javascript
self.addEventListener("message", (event) => {
    if (event.data === "PLEASE_ACTIVATE") {
        self.skipWaiting();
    }
});
```

这一次，页面没有骂人。

旧版交出岗亭钥匙，走出网络路口。Service Worker 拿着钥匙进入激活室，JavaScript 老大则在页面门口挂好交接铃：

```javascript
navigator.serviceWorker.addEventListener(
    "controllerchange",
    () => location.reload()
);
```

页面盯着铃铛等了半天，一点动静也没有。

“钥匙不是已经交了吗？”

浏览器老大指了指激活室：“人还没出来。旧仓没清完之前，这次交班就不算结束。”

## 等等，铲错了！

Service Worker 开来铲车，准备在激活时把旧缓存全部推走。

铲斗刚举起来，三扇卷帘门同时打开。

```text
shop-static-v1
shop-pages-v1
photo-editor-thumbnails
```

Service Worker 扫了一眼：“旧的，全清。”

“你敢！”

第三间仓库里冲出一群缩略图，抱住铲车轮胎。

“我们是同一个源下那套图片编辑器的！”

Service Worker 愣住：“怎么会住在我的 Cache Storage 里？”

“谁说是你的？”缩略图们齐声喊，“同一个源上的仓库都在这片库区！”

浏览器老大从铲斗底下抢回操作杆：“清带 `shop-` 前缀的旧版本。别人的名字看不懂，就别碰。”

Service Worker 只好重新填写清理单：

```javascript
const CURRENT_CACHES = new Set([
    "shop-static-v2",
    "shop-pages-v2"
]);

self.addEventListener("activate", (event) => {
    event.waitUntil(
        caches.keys().then((names) => Promise.all(
            names
                .filter((name) => {
                    return name.startsWith("shop-") &&
                        !CURRENT_CACHES.has(name);
                })
                .map((name) => caches.delete(name))
        ))
    );
});
```

清仓时，请求车在岗亭外按起了喇叭。

“怎么还不开门？”

Service Worker 伸手去抬栏杆，浏览器老大用操作杆敲了敲 `event.waitUntil()`。

“新仓还没核对完，旧仓正在拆。现在放车进去，准备让它停在哪堆砖下面？”

Service Worker 只好继续等。

最后一间旧仓关闭，激活工作完成，控制者变更的铃声终于响了。

JavaScript 老大立刻让页面重新导航。请求车第一个冲进新岗亭，页面带着 v2 的脚本、v2 的样式和 v2 的控制规则重新回来，结算按钮原地蹦了两下。

“还活着。”

Service Worker 把钥匙挂到腰间：“原来 `skipWaiting()` 不是不能用，是不能闭着眼睛用。”

缩略图们冲 Service Worker 做了个鬼脸，推着自家仓库走了。

## 岗亭怎么没了？

浏览器老大重新拉下断网闸，又推进来一台刚拆封的测试机。

“最终验收。第一项，新设备首次访问。”

URL 在新机器里输入商店地址，一脚油门冲向网络路口。

前方没有亮灯。

没有栏杆。

连岗亭都没有。

URL 在黑暗里转了一圈：“Service Worker 呢？”

Service Worker 隔着原来那台机器的玻璃拼命挥手：“我在这里！”

“那是另一台机器。”HTTP 把 Service Worker 的手按了下去，“新设备以前没打开过商店，页面没机会注册你，安装室也没机会往本地仓库放资源。”

Service Worker 还想争辩：“现在注册不行吗？”

HTTP 指了指漆黑的隧道：“连页面和 `/sw.js` 都取不回来，谁替你提交注册？”

测试机上的 URL 等了半天，只能空车回来。

浏览器老大在第一项后面画了一个叉。

“第一次访问就断网，Service Worker 救不了。下一项，已经受控的页面访问一份没缓存过的资源。”

镜头切回原来的购物页面。

主人点开一件从未访问过的商品。请求车顺利停在 Service Worker 的岗亭前。

这次有岗亭，也有人值班。

Service Worker 拉开仓库门。

对应货架是空的。

Service Worker 又把请求放向网络，隧道里仍然没有信号。

URL 趴在栏杆上：“有岗亭也不行？”

“我只能交出存过的响应，或者提前准备好的离线提示。”

Service Worker 从角落取出 `/offline.html`，递给页面。

页面总算没有一片空白，却也没能显示那件商品。

HTTP 在两份验收记录中间画了一条线：

```text
从未安装：没有岗亭
已经接管但没有资源：仓库没货
```

Service Worker 盯着那条线看了一会儿，撕掉墙上“离线什么都能开”的广告。

## 这钱你也敢收？

主人把一双鞋加入购物车，点击付款。

一辆请求车停在岗亭前：

```text
POST /api/orders
```

Service Worker 看了看断网灯，准备把请求推进 Cache Storage。

按照岗亭原来的规则，非 `GET` 请求本该直接放行。可隧道已经断了，放行也只会撞进黑暗里。

仓库门“砰”地弹开，把车顶了回来。

```text
只接收 GET 请求
```

Service Worker 揉着鼻子：“要不先回一个付款成功？网络恢复后再补订单。”

HTTP 一把夺过响应纸：“你敢替服务器把钱收了？”

“先让页面高兴一下。”

“库存谁扣？”

“以后扣。”

“身份谁验？”

“以后验。”

“网络恢复后鞋卖完了呢？”

Service Worker 不说话了。

JavaScript 老大推来一只写着 `IndexedDB` 的保管箱：“确实需要离线提交，可以先把待办数据放这里。但页面必须说清楚——只是等待提交，不是已经成功。”

保管箱接过订单，贴上一张黄色纸条：

```text
待发送
```

HTTP 又补了一张：

```text
重试时小心重复订单
```

Service Worker 盯着两张纸条：“离线买双鞋，怎么比在线还多两道手续？”

“因为服务器不在。”HTTP 把“付款成功”的假响应撕成两半，“你可以保管请求，不能冒充结果。”

## 仓库还能被拖走？

验收快结束时，库区忽然响起倒车提示音。

一名存储管理员开着清理车进来，在整片库区门口挂上“存储压力清理演练”的牌子，又把拖车绳套在大门上。

Service Worker 扑过去：“里面全是我的离线库存！”

“假设整个浏览器都在喊空间不够。”存储管理员递出一张容量表，“你拿什么保证这些仓库永远留在设备上？”

“我已经存进 Cache Storage 了。”

“Cache Storage 占的还是设备空间。空间紧张，或者主人主动清理站点数据，这片库区都可能保不住。”

Service Worker 抱住仓库大门：“至少每次都提前通知我？”

存储管理员没有回答，收起拖车绳，在容量表的“无法保证”一栏打了勾。

HTTP 在验收表上写下最后一句：

```text
缓存是退路，不是承诺
```

Service Worker 看了很久，拿笔把“不是承诺”划掉，改成了四个字：

```text
不是服务器
```

“这回服了？”HTTP 问。

“只服一半。”Service Worker 把那份缓存首页递给 URL，“服务器不在时，至少这条走过的路，我还能留一盏灯。”

HTTP 接过首页，检查一遍，终于放行。

“行。灯可以亮。”

“车也可以拦？”

“按规则拦。”

“成交。”

## 服务器终于急了

隧道外的信号灯重新亮起。

积压的请求车一辆接一辆驶向服务器。商品页为了拿到新消息，每隔几秒就派 URL 跑一趟。

“有新订单吗？”

“没有。”

三秒后。

“有新订单吗？”

“没有！”

又三秒。

“现在呢？”

服务器终于拍桌子。

“每次都得等页面先问，我才能回答？”

HTTP 理了理白大褂：“请求在前，响应在后。一直是这个规矩。”

“可我有新消息时，为什么不能主动开口？”

服务器从墙里拽出一根长长的双向线路，扔到 TCP 老司机面前。

“给我接上。”

TCP 老司机没动，先从抽屉里拿出一摞表格。

最上面一张写着：

```text
长连接保活、断线重连与消息积压登记表
```

服务器的眼皮跳了一下。
