# iframe

> 内嵌框架标签，可以在页面内部内嵌一个网页，实现在一个标签页中同时显示两个网页，iframe最常用的地方时一些技术博客网站中，例如w3school网站的实例网页，左边为网页代码，右边为网页显示效果，就使用了iframe实现的。

## iframe属性

> 常用属性：
>
> * width：框架宽度
> * height：框架高度
> * title：框架的标题，用来给屏幕阅读器使用的。
> * name：框架名称，用来给`a标签`、`form标签`、`button`等标签的target属性使用，点击时将响应页面显示在框架中。
> * src：框架显示的页面地址。

```html
<html>
	<head>
    
  </head>
  <body>
    <iframe src="./iframe-simple-contents.html" title="iframe example 1" width="400" height="300" name="sonIframe">
  <p>Your browser does not support iframes.</p>
</iframe>
  </body>
</html>
```

## 外层页面操作iframe页面

> 外层页面访问内层页面必须遵守同源规则，在同一个域名中，否则无法得到iframe页面的document对象和window对象
>
>  
>
> 获取iframe框架的window对象方式：
>
> 1. 使用获取iframe元素节点，使用contentWindow属性获取iframe页面的window对象。 
> 2. 页面中所有iframe标签生成的window对象都会保存在外部页面window对象中的`frames属性`中，使用frames[0]获取。
>
> ​		
>
> 使用contentDocument属性获取iframe页面的document对象。

```html
<!DOCTYPE html>
<html>

<head>
    <style>
        iframe{
            background-color: red;
        }    
    </style>
</head>

<body>
    <div>adsfadsfdf</div>
    <iframe src="#" frameborder="0"></iframe>

    <script>
        // 获取iframe框架中window对象方式
            var window1 = window.frames[0]; // 方式1
            var window2 = document.querySelector('iframe').contentWindow; // 方式2
            console.log(window1 === window2);
        // 获取iframe框架中document对象方式
            var document1 = window.frames[0].document; // 方式1
            var document2 = document.querySelector('iframe').contentDocument; // 方式2
            console.log(document1 === document2);
    </script>
</body>

</html>
```

