# link标签

> link标签是一个文件关联标签，常用来关联css文件，当然也可以关联其它类型文件，例如图片、视频、音频等。

```html
<!DOCTYPE html>
<html>

<head>
  	<!-- 关联css文件 -->
    <link rel="stylesheet" href="./link1.css" type="text/css">
  	<!-- 关联图标-->
    <link rel="icon" href="a1920.jpg" type="image/png">
</head>

<body>
    <div></div>
</body>

</html>
```

## link中常用的属性

> * rel：该属性指定当前文件与关联的文件的关系，例如引入css文件时为stylesheet(样式表)
> * href：关联文件的地址
> * type：关联文件的MIME类型
> * media：关联文件时设置媒体查询条件，当满足某些条件时才可以关联该文件。
>
> 下面的例子用来解释media的用法：
>
> ​		当屏幕宽度小于600px时网页使用css1文件样式，当屏幕宽度在600～800px之间时，网页使用css2文件的样式。

```css
/* css1.css 文件*/
div{
    background-color: red;
    width: 300px;
    height: 300px;
}
```

```css
/* css2.css 文件*/
div{
    background-color: green;
    width: 600px;
    height: 600px;
}
```

```html
<!DOCTYPE html>
<html>

<head>
  	<!-- 当视口宽度小于600px时，网页按照css1文件中的样式显示 -->
    <link rel="stylesheet" href="./css1.css" media="(max-width:600px)" type="text/css">
  	<!-- 当视口宽度在601px～800px之间时，网页按照css1文件中的样式显示 -->
    <link rel="stylesheet" href="./css2.css" type="text/css" media="(max-width:800px) and (min-width:601px)">
</head>

<body>
    <div>adsfadsfdf</div>
</body>

</html>
```

## 设置标签页中的图标

> 使用link加载图片，rel属性为icon。

```html
<!DOCTYPE html>
<html>

<head>
		<!-- 图标将连接的图标 -->
    <link rel="icon" href="a1920.jpg" type="image/jpg">
</head>

<body>
</body>

</html>
```



## 设置苹果手机投屏图标

> 需求：浏览器中某些网站比较好，想要将其当成一个app来使用，放在桌面上当成app来使用。使用link标签

```html
<!DOCTYPE html>
<html>

<head>
		<!-- 作为iphone手机的app图标 -->
    <link rel="apple-touch-icon" href="/logo192.png" />
</head>

<body>
</body>

</html>   

```

![perserveSelf02](media/link标签/perserveSelf02.gif)



## 应用加壳

> 给web应用配置手机上app应用的一些图标和配置信息。
>
> 想看详细manifest文件配置，可以使用HBuilder软件进行设置，或者百度搜索，等待日后完善。

```html
<link rel="manifest" href="/manifest.json" />
```

