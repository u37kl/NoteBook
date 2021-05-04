# [3分钟搞懂FastClick原理解析](https://segmentfault.com/a/1190000023617219)

### 为什么要用FastClick

在移动端H5开发过程中，关于点触可能会遇到如下两个问题：

- 手动点击与真正触发`click`事件会存在 300ms 的延迟
- 点击穿透问题（点击行为会穿透元素触发非父子关系元素的事件）

延迟的存在时因为浏览器想知道你**是否在进行双击**操作；而点击穿透是因为 300ms 延迟触发时的副作用。

### 原理过程

```
  // 业务代码
  var $test = document.getElementById('test')
  $test.addEventListener('click', function () {
    console.log('1 click')
  })

  // FastClick简单实现
  var targetElement = null
  document.body.addEventListener('touchstart', function () {
    // 关键点1：记录点击的元素
    targetElement = event.target
  })
  document.body.addEventListener('touchend', function (event) {
    // 关键点2：阻止默认事件（屏蔽之后的click事件）
    event.preventDefault()
    var touch = event.changedTouches[0]
    // 关键点3：合成click事件，并添加可跟踪属性forwardedTouchEvent
    var clickEvent = document.createEvent('MouseEvents')
    clickEvent.initMouseEvent('click', true, true, window, 1, touch.screenX, touch.screenY, touch.clientX, touch.clientY, false, false, false, false, 0, null)
    clickEvent.forwardedTouchEvent = true // 自定义的
    targetElement.dispatchEvent(clickEvent)
  })
```

原理说明：
在document.body 上绑定 touchstart 和 touchend
其中，touchstart 用于记录当前点击元素的targetElement。
touchend 用于：

- 组织默认事件（屏蔽之后的click事件）
- 合成click事件，并添加可跟踪属性forwardedTouchEvent
- 在 targetElement 上触发 click 事件
- targetElement 上绑定的事件立即执行！done

### 点击穿透

比如，页面上有A和B元素，B在A上面。在B元素的touchstart事件上注册了一个回调函数，该回调函数的作用是隐藏B元素。我们发现，当我们点击B元素，B元素被隐藏了，随后，A元素触发了click事件。
![image.png](media/007-fastClick解决300ms延时/bVbLf0W.png)
移动端上面，事件执行顺序是：touchstart > touchend > click。而 click 事件有300ms延迟，浏览器触发了click事件，但是此时B元素不见了，所以该事件被派发到了A元素身上。如果A元素是一个链接，那此时页面就会意外地跳转。

### 其他解决方案

**禁用缩放**
当HTML文档头部包含如下`meta`标签时：

```
<meta name="viewport" content="user-scalable=no">
<meta name="viewport" content="initial-scale=1,maximum-scale=1">
```

表明这个页面是不可缩放的，那双击缩放的功能就没有意义了，此时浏览器可以禁用默认的双击缩放行为并且去掉300ms的点击延迟。

这个方案有一个**缺点**，就是必须通过**完全禁用缩放**来达到去掉点击延迟的目的，然而完全禁用缩放并不是我们的初衷，我们只是想禁掉默认的双击缩放行为，这样就不用等待300ms来判断当前操作是否是双击。但是通常情况下，我们还是希望页面能通过双指缩放来进行缩放操作，比如放大一张图片，放大一段很小的文字。



## fastClick使用地方

> iOS WebView 就有点让人头疼了。因为 iOS 8 之前一直都是 UIWebView，iOS 8 出了个新秀 WKWebView，那么 iOS 9.3 300ms 延迟的 BUG 修复到底干了啥呢？在客户端 iOS 小姐姐的帮助下，最终的测试结果是 UIWebView 300ms 延迟的问题到现在一直存在，哪怕是最新的 iOS 版本（这大概这就是为什么老外推荐使用 WKWebView 而非 UIWebView，估计是不想修 BUG 了吧 ），但是 WKWebView 在 iOS 9.3 的时候将这个问题给修复了。也就是说 iOS 9.3 之前 WKWebView 仍然是存在 300ms 延迟的问题的（忙活了半天，总算把所有的都给理清楚了 ）。
>
> 回眸历史，不可否认 fastClick 在解决移动端 300ms 延迟的问题上的确作出杰出的贡献，不过 9102 的今天，是否仍然有必要使用呢，回到开始，我说过，适合自己的才是最好的，因此，如果你的业务需求，是只需要对 iOS 9.3 以上的 WKWebView 做适配，那么强烈建议你不去使用，毕竟减少了文件请求大小，引入风险的概率。