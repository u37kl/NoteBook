# 认识Link Preload 标签

## [前端性能优化 - Resource Hints 资源预加载](https://link.zhihu.com/?target=https%3A//juejin.im/post/5a52d5d06fb9a01cbd586eb6)

**Preload**:

```text
<link rel="preload" href="test.jpg"> 
```

**Prefetch**:
Prefetch包括资源预加载、DNS预解析、http预连接和页面预渲染。

```text
资源预加载：<link rel="prefetch" href="test.css">
DNS预解析：<link rel="dns-prefetch" href="//haitao.nos.netease.com">
http预连接：<link rel="prefetch" href="//www.kaola.com"> 将建立对该域名的TCP链接
页面预渲染：<link rel="prerender" href="//m.kaola.com"> 将会预先加载链接文档的所有资源
```

那么**Prefetch**和**Preload**有什么区别呢？
具体来讲，**Preload**来告诉浏览器预先请求当前页需要的资源，从而提高这些资源的请求优先级。比如，对于那些本来请求优先级较低的关键请求，我们可以通过设置**Prefetch**来提升这些请求的优先级。
**Prefetch**来告诉浏览器用户将来可能在其他页面（非本页面）可能使用到的资源，那么浏览器会在空闲时，就去预先加载这些资源放在http缓存内，最常见的dns-prefetch。比如，当我们在浏览A页面，如果会通过A页面中的链接跳转到B页面，而B页面中我们有些资源希望尽早提前加载，那么我们就可以在A页面里添加这些资源**Prefetch**，那么当浏览器空闲时，就会去加载这些资源。
所以，对于那些可能在当前页面使用到的资源可以利用**Prefetch**，而对一些可能在将来的某些页面中被使用的资源可以利用**Preload**。如果从加载优先级上看，**Prefetch**会提升请求优先级；而**Preload**会把资源的优先级放在最低，当浏览器空闲时才去预加载。





Preload 标签是什么

Preload 作为一个新的web标准，旨在提高性能和为web开发人员提供更细粒度的加载控制。Preload使开发者能够自定义资源的加载逻辑，且无需忍受基于脚本的资源加载器带来的性能损失。

```text
<link rel="preload"> 
```

## 类似的技术

关于预加载，目前已经有几种方案

## DNS prefetch

DNS prefetching通过指定具体的URL来告知客户端未来会用到相关的资源，这样浏览器可以尽早的解析DNS。比如我们需要一个在http://example.com的图片或者视频文件。在“就可以这么写：

```text
<link rel="dns-prefetch" href="//example.com"> 
```

## Preconnect

和DNS prefetch类似，preconnect不光会解析DNS，还会建立TCP握手连接和TLS协议（如果需要）。用法如下：

```text
<link rel="preconnect" href="http://example.com">  
```

## Prefetch

当能确定网页在未来一定会使用到某个资源时，开发者可以让浏览器提前请求并且缓存好以供后续使用。prefetch支持预拉取图片、脚本或者任何可以被浏览器缓存的资源。

```text
<link rel="prefetch" href="image.png"> 
```

## Subresource

subresource可以用来指定资源是最高优先级的。比如，在Chrome和Opera中我们可以加上下面的代码：

```text
<link rel="subresource" href="styles.css">  
```

## Prerender

prerender是一个重量级的选项，它可以让浏览器提前加载指定页面的所有资源。

```text
<link rel="prerender" href="/thenextpage.html"/>
```

## Preload 的优势

相对于类似的技术，Preload 有 as 属性，这让浏览器可做一些 subresource 和 prefetch 无法实现的事：

- 浏览器可以设置正确的资源加载优先级
- 浏览器可以确保请求是符合内容安全策略的
- 浏览器能根据 as 的值发送适当的 Accept 头部信息
- 浏览器通过 as 值能得知资源类型

Preload 的与众不同还体现在 onload 事件上（在 Chrome 中，prefetch 和 subresource 是不支持的）。preload 不会阻塞 windows 的 onload 事件，除非，preload资源的请求刚好来自于会阻塞 window 加载的资源。

## Preload 用法举例

```text
<link rel="preload" href="a.js" as="script">
```

忽略 as 属性，或者错误的 as 属性会使 preload 等同于 XHR 请求，浏览器不知道加载的是什么，
因此会赋予此类资源非常低的加载优先级。

## Preload 应用

## 对字体的提前加载

字体被隐藏在css中间，浏览器有时候不能很好的将他们放入预加载器中，为此我们可以借助preload

```text
<link rel="preload" href="font.woff" as="font" type="font/woff" crossorigin>
```

crossorigin 属性是必须的，即便是字体资源在自家服务器上，因为用户代理必须采用匿名模式来获取字体资源。

## 基于标记语言的异步加载

css 异步加载

```text
<link rel="preload" as="style" href="asyncstyle.css" onload="this.rel='stylesheet'">
```

js 异步加载

```text
<link rel="preload" as="script" href="async_script.js" onload="var script = document.createElement('script'); script.src = this.href; document.body.appendChild(script);">
```

async 属性会阻塞 window 的 onload 事件，这么写就没有问题了。

## 动态加载，但不执行

```text
var link = document.createElement("link");
link.href = "myscript.js";
link.rel = "preload";
link.as = "script";
document.head.appendChild(link);
var script = document.createElement("script");
script.src = "myscript.js";
document.body.appendChild(script); 
```

## 检测是否支持 preload

用下面的代码段可以检测是否被支持：

```text
const preloadSupported = () => {
    const link = document.createElement('link');
    const relList = link.relList;
    if (!relList || !relList.supports)
        return false;
    return relList.supports('preload');
};
```

## Preload 在生产环境的案例

## Treebo 案例

Treebo，印度最大的旅馆网站之一，在 3G 网络下对其桌面版试验，在对其顶部图片和主要的 Webpack 打包文件使用 preload 之后，在首屏绘制和可交互延迟分别减少了 1s。

同样的，在对自己的渐进式 Web 应用程序主要打包文件使用 preload 之后，Flipkart 在路由解析之前 节省了大量的主线程空闲时间（在 3G 网络下的低性能手机下）。