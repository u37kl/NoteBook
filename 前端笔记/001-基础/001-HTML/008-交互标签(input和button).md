# 交互标签

> input标签：
>
> * input-checkbox：复选框
> * input-radio：单选框
> * input-text：文本输入框
> * input-password：密码输入框
> * input-file：文件
> * input-date：日期
> * Input-color：颜色选择器. ---`H5新增`
> * input-range：范围选择器 --- `H5新增`
> * input-submit：提交按钮
> * input-reset：重置按钮
> * input-button：普通按钮
>
> 按钮：Button
>
>  
>
> input标签写法：
>
> 1.  <input type="checkbox" name="name" id="checkBox" value="checkBoxValue">
> 2.  <input type="radio" name="radio" id="radio", value="radioValue">
> 3.  <input type="text" name="text" id="text" placeholder="请输入姓名">
> 4.  <input type="password" name="pass" id="pass">
> 5.  <input type="file" name="file" id="file">
> 6.  <input type="date" name="date" id="date">
> 7.  <input type="color" id="myColor">
> 8.  <input type="range" id="myRange">
> 9.  <input type="submit" value="提交">
>
> 



## 记录信息的input标签

>* input-checkbox：复选框
>* input-radio：单选框
>* input-text：文本输入框
>* input-password：密码输入框
>* input-file：文件
>* input-date：日期
>* Input-color：颜色选择器 
>* input-range：范围选择器

### 单选框

> 1. 必须让所有单选框的name相等才能出现单选效果，否则不会出现单选效果。
> 2. 使用单选框的id属性给label标签的for属性赋值，实现label与单选框关联，点击label时单选框选中。
> 3. 设置`checked属性`，表示该单选框已经选中，浏览器会保证只能由一个单选框是选中状态。

```html
<!-- 定义了一个性别单选框，初始值为选中男性-->  
<input type="radio" name="sex"  value="man" id="sex1" checked><label for="sex1">男</label>
<input type="radio" name="sex" value="woman" id="sex2"><label for="sex2">女</label><br>
```

### 复选框

> 1. 设置`checked属性`，表示该单选框已经选中。
> 2. 所有复选框的name相等，保证表单提交时，看成一个整体提交。
> 3. 设置`checked属性`，表示该复选框已经选中。
>
> 复选框值的提交方式(选择篮球+足球)：`ball=basketball&ball=football`

```html
<input type="checkbox" name="ball"" id="baseball" value="baseball"> <label for="baseball">棒球</label>
<input type="checkbox" name="ball" id="basketball" value="basketball"> <label for="basketball">篮球</label>
<input type="checkbox" name="ball" id="football" value="football"> <label for="football">足球</label><br>
```

### 输入框(文本、密码、搜索等)

> html5新增属性：
>
> * required：当前必须填写，当表单提交时，浏览器会检查是否填写了。
> * readonly：只读，无法修改
> * pattern：正则表达式，判断输入内容合法，当表单提交时，浏览器会检查是否合法。
> * placeholder：提示文本
> * maxlength：限制输入内容字数，当表单提交时，浏览器会检查是否字数超出。
>
> minlength：限制输入内容字数，当表单提交时，浏览器会检查字数是否少于规定。

```html
// 普通文本框
<input type="text" name="text" id="text" placeholder="请输入姓名"><br>
// 密码文本框
<input type="password" name="pass" id="pass" maxLength="6" minlength="4"><br>
// 电话文本框
<input type="tel" name="telephone" id="telephone">
// 搜索文本框
<input type="search" name="search" id="search">
// email文本框
<input type="email" name="email" id="email">
```



### 文件

> multiple(html5新增属性)：是否允许选中多个问题
>
> accept：该属性只能使用在`input文件标签`中，限制用户选择的文件类型。

```html
<input type="file" name="file" id="ff" multiple>


<!-- 
    只有三种文件类型：image、audio、video。选择多个时使用逗号隔开
-->
<!-- 只允许用户选择jpg图片 -->
<input type="file" name="file" id="ff" multiple accept="image/jpg"><br>
<!-- 只允许用户选择jpg图片或者所有的音频文件 -->
<input type="file" name="file" id="ff" multiple accept="image/jpg,audio/*"><br>
<!-- 只允许用户选择图片、音频文件和视频文件 -->
<input type="file" name="file" id="ff" multiple accept="image/*,audio/*,vedio/*"><br>
```

### 日期和数字

> html5新增属性：
>
> * max：限定输入的最大数字或者日期
> * min：限定输入的最小数字或者日期

```html
// 日期只能输入2000-05-02 到 2020-11-12之间的值。
<input type="date" name="currentDate" id="date" min="2000-05-02" max="2020-11-12">

// 数字只能选择器5～8之间的值
<input type="number" name="num" id="num" min="5" max="8">
```



### 范围选择器-H5新增

> <input type="range" min="0" max="8"  step="4">
>
> `min`：范围最小值
>
> `max`：范围最大值
>
> `step`：每次移动滑块时的步幅。



## 按钮标签

> html两种按钮：
>
> 1. Input，使用value值设置按钮上的文本。
> 2. button，标签可以包含子元素。
>
> 两种按钮的区别：
>
> ​		button可以添加子元素，而input-submit无法添加子元素，因此在按钮样式上button会比input-submit更丰富。
>

### input按钮标签

> input有三种按钮，分别是：
>
> * input-submit，提供表单提交功能，向服务器发送表单数据
> * input-reset，重置表单，清空表单元素中输入的数据。
> * input-button，普通的按钮功能，提供一个点击变色的效果，点击没有任何作用。

```html
    <form action="./index.html" method="get">
    		<!-- 必须填写 -->
        <input type="tel" name="telephone" id="telephone" required> <br>

      	<!-- 修改form提交时的地址和http请求方式-->
        <input type="submit" value="提交" formaction="./test.html" formmethod="post"><br>
	      <input type="reset" value="重置"><br>
      	<input type="button" value="按钮">
      	
      	<!--跳过表单验证-->
        <button formnovalidate>按钮</button>
    </form>
```

### button标签

> button标签有三种样式：
>
> * type属性=button，默认样式，只是一个普通按钮。
> * type属性=submit，提交按钮，放在form表单中，用户点击按钮时会将表单中的数据提交到服务器。
> * type属性=reset，重置按钮，放在form表单中，用户点击按钮时会清空表单中的数据。
>



### 按钮与表单相关的属性

> html5新增属性(个别浏览器不兼容IE9及以下)：
>
> * formaction：指定访问服务器的url，覆盖form标签中设置的值
> * formenctype：指定数据格式，覆盖form标签中设置的值
> * formmethod：指定http请求方式，覆盖form标签中设置的值
> * formtarget：指定页面打开位置，覆盖form标签中设置的值
> * formnovalidate：跳过表单验证，直接向服务器发送数据，safari不支持该属性

## 常用属性

> 1. disabled：是否允许交互
> 2. name：表单提交该控件的数据时，作为json中的key
> 3. value：表单提交该控件的数据时，作为json中的value
> 4. required：除了image、hidden、submit类型的input标签外的其他input标签支持，必须给控件添加数据。
> 5. readonly：只读无法修改。
>
> 
>
> html5新增：
>
> 1. autofocus：html文件加载完成后自动获取焦点，即当网页显示时，自动选中，一般用在填写一些信息的页面中，用户不需要选中控件再去输入了，直接输入就行。
> 2. form：指定当前控件所属的form表单，form值为form表单的id属性值
> 3. autocomplete：针对输入框的属性，属性值为on时，每当输入时会显示输入过的历史数据，off时，输入时不会显示任何历史数据。