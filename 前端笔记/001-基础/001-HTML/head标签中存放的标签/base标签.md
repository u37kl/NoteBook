# base标签

> 用于定义网页在跳转时的根url和target。使用方式：
>
> 1. 当这些可以发送请求的标签(a标签、form标签等)指定的url为相对路径时，会将base标签中的href与其进行拼接然后请求。
> 2. 当这些可以发送请求的标签(a标签、form标签等)指定的url为绝对路径时，忽略base标签的href。
>
> 如果这些可以发送请求的标签没有指定target时，使用base标签指定的target。

```html
<!DOCTYPE html>
<html>

<head>
    <base href="https://www.baidu.com" target="_blank">
</head>

<body>
    <!-- 点击a标签，
        href中没有设置属性值时，会使用base标签中href定义的属性值作为地址请求。
        target属性页没有指定时，会使用base标签中的target属性值。
    -->
    <a href="">点击</a>

    <!-- href使用相对路径时，会将base标签和a标签的href拼接，https://www.baidu.com/firstPage.html#divs -->
    <a href="firstPage.html#divs"></a>

    <!-- a标签使用绝对路径时，不在使用base标签的href了。 -->
    <a href="https://www.jd.com"></a>
</body>

</html>
```

## 页面出现多个base标签

> 当页面出现多个base标签时，编写顺序的第一个base标签时有效的，浏览器会忽略后面所有的base标签，使用第一个base标签的href和target属性。

## <base>中的href 适用于src吗？

> 适用所有提交请求的标签，form表单中的action属性、img标签中src属性、a标签中的href属性，source标签中的src属性，只要这些标签的属性使用相对路径，就会将其和base标签href属性进行拼接。