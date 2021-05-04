# jQuery

> jQuery是基于javascript语言开发的框架。
>
> jQuery目前有三种版本：
>
> 1. 1.xx，文件比较大， 但是兼容老版IE
> 2. 2.xx，文件相对1版本小，执行效率高，适配部分IE8特性，IE8及以下版本使用2版本会有一些问题。
> 3. 3.xx，文件相对2版本小，删除了ajax和动画的API。
>
> jQuery的两种类型：
>
> 1. 非压缩版
> 2. 压缩版，删除非压缩版中的注释代码，和所有的格式化空格和换行。
>
> jQuery的引入方式：
>
> 1. 本地引入，将jQuery文件放在本地服务器中，但是每次访问时都需要向服务器中下载该文件，大大增加了服务器的负担。
> 2. 远程引用，使用CDN远程库中的jQuery文件，每次访问时不在向服务器中下载，减少服务器访问压力。
>
> 
>
> 实践中，基本不用2和3版本的jQuery，因为不兼容IE老版本。

## jQuery核心函数

> jQuery是一个函数对象，JS函数对象的特点就是既可以当函数使用，又可以当成不同对象使用。所有的功能都是绑定在函数对象中。
>
> jQuery实现原理：
>
> 使用匿名函数自调用形式，创建一个函数表达式，并将函数表达式绑定到window对象中，通过这个函数表达式操作jQuery库中提供的功能。

```javascript
console.log(typeof $ == 'function');  // true
console.log($ === jQuery, window.$ === $, window.jQuery = jQuery);  // true true true
```

### jQuery核心函数作为普通函数使用

> 核心函数可以接收四种类型参数，不同参数功能不一样。
>
> 核心函数接收的四种类型参数：
>
> 1. 回调函数，开启一个DOM监听，当DOM加载完毕后，执行回调函数，可以部分代替window.onload功能。
> 2. DOM对象，将DOM对象封装成jQuery对象，调用jQuery方法。
> 3. `css选择器，最常用的方式，查找匹配的DOM对象，封装成jQuery对象`。
> 4. html代码字符串，将html代码解析成DOM对象并封装成jQuery对象。
>
> jQuery对象是一个伪数组，它会像数组一样，以数字作为属性名保存DOM对象，当DOM对象转成jQuery对象时，会以index-DOM对象形式保存在jQuery对象中。
>
>  
>
> 使用格式：($是jQuery库中保存函数对象的变量名，绑定在window中)
>
> 		*  $(实参);    // 最常用的方式
> 		*  jQuery(实参);
> 		*  window.$(实参)
> 		*  window.jQuery(实参)
>
> ​		



#### 参数为回调函数

> ​		下面代码执行结果是先打印bbb在打印aaa。
>
> 
>
> ​		$(function(){})这种方式，当DOM加载完毕后，才会执行回调函数中的代码，类似于 window.onload，因此所有DOM操作必须放在这个回调函数中，或者放在window.onload方法中。

```html
<html>
	<body>
    <script>
    		$(function(){
          console.log('aaa');
        });
      
      	console.log('bbb');
    </script>
  </body>
</html>
```

#### 参数为DOM对象

> ​	下面代码就是将div元素节点转换成jQuery对象，这样就可以调用jQuery库提供的方法。注释中展示了jquery对象的数据结构，它是一个伪数组，并不是真正的数组。

```javascript
/**
* DOM对象转成jQuery对象的数据解构
* $div: init(1){
*    0: div.class1,       // 保存当前DOM对象
*    context: document,   // 当前DOM对象所在的document对象
*    length: 1,           // 当前伪数组中保存的DOM对象数
*    selector: "div"      // 选择器
*    __proto__: Object(0)
* }
*/
let div = document.querySelector('.class1');
let $div = $('div');
console.log($div);
```

#### 参数为css选择器

> ​	这种使用方式是最常用的，jquery有着非常强大的查询元素节点的功能，底层还是调用的document.querySelectorAll()函数实现的查询功能，并将查询到的DOM对象保存到jquery对象中。

```html
<html>
	<body>
    	<div class="div1">
      		<span>1111</span>  
        	<span>2222</span>
    	</div>
    
    <script>
    		// 使用css查询div中第二个span节点。
      	// JS原生方式
      	let span = document.querySelector('.div1 span:nth-of-type(2)');
        span.style.backgroudColor = 'red';
      
      	// jQuery方式，查询加赋值
      	let$span = $('.div1 span:nth-of-type(2)').css('backgroundColor', 'red');
    </script>
  </body>
</html>
```

#### 参数为html代码

> ​	这种方式能更简便快速的创建DOM对象。

```html
<html>
	<body>
    	<div class="div1">
      		<span>1111</span>  
    	</div>
    
    <script>
    		// 在div下创建span标签。
      	// JS原生方式
				let divSon = document.createElement('div')
				divSon.innerHTML = '<span>222</span>';
			  divSon.style.backgroundColor = 'red';
		    document.querySelector('.div1').appendChild(divSon);
      
      	// jQuery方式
      	let$div = $('<div><span>222</span></div>').css('backgroundColor', 'red');
      	$('.div').append($div);
    </script>
  </body>
</html>
```

## jQuery对象

> jQuery对象，由jQuery核心函数创建的对象，一个包裹DOM元素的伪数组，通过操作jQuery对象，实现对伪数组中DOM元素的的操作，例如css样式修改、DOM树结构改变(增删改)。
>
>  
>
> jQuery对象数据结构：
>
> 
>
> $div: init(1){
>
> ​    0: div.class1, // 保存当前DOM对象
>
> ​    context: document, // 当前DOM对象所在的document对象
>
> ​    length: 1, // 当前伪数组中保存的DOM对象数
>
> ​    selector: "div" // 选择器
>
> ​    __proto__: Object(0)
>
> }
>
>  
>
> 一定要分清`$`和`$('div')`的区别，前者是一个函数对象，后者是该函数的调用，返回一个jQuery对象

### jQuery选择器

> jQuery库中最常见的生成jQuery对象的方式，就是使用jQuery核心函数通过选择器筛选DOM对象，并封装成jQuery。jquery选择器的原理其实是底层调用了document.querySelectorAll方法来实现的。

```javascript
// id选择器
$('#idName')
// class选择器
$('.className')
// 群组选择器
$('p, a, div')
// 后代选择器
$('div p')
// 子代选择器
$('div > p')
// 临近选择器
$('p + span')
// 兄弟选择器
$('p ~ span')
// 属性选择器
$('input[type=text]')
$('input[type=text]')
$('li[title!=hello]') // 选择文档中title属性值不为hello的li标签

// 伪类选择器
$('div p:first') // 找出页面中第一个包裹在div中的p标签，即第一次写在div中的p标签。
$('div p:last') // 找出页面中最后一个包裹在div中的p标签，即最后一次写在div中的p标签。
$('td:even') // 选择奇数索引td标签
$('td:odd') // 选择偶数索引td标签
$('p:contains("aaa")') // 选择p标签中包含aaa文本的p标签。

$('td:eq(2)') // 相当于document.querySelectorAll('td:nth-of-type(3)') // eq是从0开始计数，nth-of-type从1开始计数
$('td:lt(4)') // 选择td标签集合中下标 < 4的元素
$('td:gt(2)') // 选择td标签集合中下标 > 2的元素
$('td:gt(2):lt(4)') // 选择td标签集合中下标 > 2的元素和 < 4的元素

$('input[type=checkbox]:not(:checked)') // 选择哪些没有选中的复选框标签
$('div:not(.box)') // 选择所有class值不为box的div。
```

### 表单选择器

>$('input')和$(':input')的区别：前者是筛选文档中所有的input标签，后者是筛选文档中所有的表单元素标签(input、button、textArea、select)

```javascript
$(':input') // 文档中的选择表单标签元素，input、button、textArea、select
$(':text') // 选择文档中所有的文本框标签
$(':password') // 选择文档中所有的密码框标签
$(':radio') // 选择文档中所有的单选框标签
$(':checkbox') // 选择文档中所有的复选框标签
$(':submit') // 选择文档中所有的提交按钮标签
$(':image')  // 选择文档中所有的图片框标签
$(':reset')  // 选择文档中所有的重置按钮标签
$(':button') // 选择文档中所有按钮标签
$(':file')  // 选择文档中所有的文件框标签
$(':hidden') // 选择文档中所有的隐藏标签

// 表单状态属性
$(':enabled') // 选择文档中所有的可交互标签，input、select、textArea、button
$(':disabled') // 选择文档中有属性为disabled="disabled"的标签
$(':checked')  // 选择文档中所有处于选中状态的单选框和复选框
$(':selected') // 选择文档中所有处于选中状态的下拉框。


// 获取下拉框中所有选项
$('select option')
// 提取下拉框中选中的那一项
$('select option:selected')
// 选中不可使用的表单元素标签
$(':input:disabled')
```



### jQuery选择器是如何运行的

> ​		选择器的原理其实是一个一层层过滤，从大集合中选中符合条件的子集合，例如`div:first`，先查出页面所有div形成一个集合，然后从该集合中选中符合条件的子集合，第一个div。

## jQuery对象与DOM对象的转换

> DOM对象转jQuery方法，使用jQuery核心函数，`$(DOM对象)`。
>
> jQuery转DOM对象方法：
>
> 1. jQuery对象.each(function(ele, index){})，在回调函数中获取jquery对象中保存的DOM对象(this和ele都指向遍历出来的DOM对象)。
> 2. jQuery对象是一个伪数组，因此可以使用中括号访问其中的DOM对象，jquery对象[index]。



## jQuery编写习惯

> 所有保存jquery生成对象的变量，变量名前面要加`$`，方便在阅读代码时，快速区分哪些是jquery变量，哪些是JS原生代码生成的变量，防止使用原生对象调用jquery中的方法的情况发生。

