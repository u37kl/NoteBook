# label

> label为说明标签，常常与input标签组合使用，用来说明当前input接收什么值。
>
> 使用label好处：
>
> 1. 让用户从视觉上清晰的了解该文本框用来干啥，对于屏幕阅读器，可以读取label中的值。
> 2. 扩大input交互范围，当label与input绑定后，点击label时，与其关联的input会获得焦点。

```html
<form action="#" method="GET">
    <label for="name">姓名</label>
    <input type="text" name="name" id="name" autocomplete="on"><br>
    <label for="age">年龄</label>
    <input type="text" name="age" id="age"><br>
    <label for="address">地址</label>
    <input type="text" name="address" id="address"><br>
</form>
```



# fieldset

> 该标签会和legend配合使用，对表单中的标签进行分组。

```html
<form action="#" method="GET">
    <!-- 对表单进行分组 -->
    <fieldset>
        <!-- 使用legend标签设置该组的标题 -->
        <legend>基本信息</legend>
        <label for="name">姓名</label>
        <input type="text" name="name" id="name" autocomplete="on"><br>
        <label for="age">年龄</label>
        <input type="text" name="age" id="age"><br>
        <label for="address">地址</label>
        <input type="text" name="address" id="address"><br>
    </fieldset>
</form>
```

## H5新增属性

> name：分组名称，没啥用，分组的标题有legend标签的内容决定。
>
> disabled：该组中的所有表单元素不可交互。
>
> form：指定当前分组属于哪个表单，一般用在fieldset标签不在form表单中的时候。



# progress

> 进度条标签，IE9及以下版本的浏览器不支持。
>
> progress存在两个属性：
>
> * max：进度条最大值。
> * value：当前进度条进度。

```html
<progress max="100" value="10"></progress>
```