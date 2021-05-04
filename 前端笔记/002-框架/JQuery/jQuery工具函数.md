# jQuery工具函数

> $.each：jQuery提供的通用遍历方法，可以变量任何类型。
>
> $.trim：jQuery提供的字符串处理方法，去除字符串前后空格。
>
> $.extend：给存在的实例对象添加属性
>
> $.grep：使用过滤函数过滤数组元素。
>
> $.inArray：确定第一个参数在数组中的位置，从0开始计数(如果没有找到则返回 -1 )。
>
> $.toArray：把jQuery集合中所有DOM元素恢复成一个数组。
>
> $.merge：合并数组
>
> $.unique：合并数组并排重复，只能处理DOM元素的数组
>
> $.contains：一个DOM节点是否包含另一个DOM节点。
>
> $.type：检测obj的数据类型

## 基础功能

> each、trim、grep、inArray、merge、type

```html
<script src="../jquery-1.12.4.js" type="text/javascript"></script>
<script>

console.log('-------------------------each------------------------------');
// each工具方法，遍历js对象
var arr = ['aaa', 'bbb', 'ccc', 'ddd'];
$.each(arr, function(index, ele){
console.log(index, ele);
});

// jquery对象的each方法遍历其中的元素，底层也是调用$.each函数
$('.inner').each(function(index, ele){
console.log(index, ele.innerHTML);
});

console.log('-------------------------trim------------------------------');
var str = ' abc def ';
var newStr = $.trim(str);
console.log(str);
console.log(newStr);


console.log('-------------------------grep------------------------------');

// jQuery提供的数组过滤函数，三个参数：数组，过滤函数，布尔值。true表示返回的数组为不满足条件的元素，默认为false，表示返回满足条件的元素。
var arr1 = [1, 2, 3, 4, 5];
var arr1_new = $.grep(arr1, function(ele, index){
return ele > 3;
});
console.log(arr1_new);
arr1_new = $.grep(arr1, function(ele, index){
return ele > 3;
}, true);
console.log(arr1_new);

console.log('-------------------------inArray------------------------------');
// 寻找元素在数组中的位置，没有则返回-1，参数：查询元素，数组，从第几个下标寻找
var arr1 = ['aaa', 1, 2, 14, true, 'bbb']
console.log($.inArray('aaa', arr1), $.inArray('aaa', arr1, 1));

console.log('-------------------------merge------------------------------');
// 将第二个数组中的元素合并到第一个数组中，不排重
var arr1 = ['aaa', 1, 2, 14, true, 'bbb']
var arr2 = [1, 2, 4, 5, 6];
var arr3 = $.merge(arr1, arr2)
console.log(arr1, arr2, arr3);

console.log('-------------------------type------------------------------');
// 判断数据类型
console.log($.type(12));
console.log($.type('123'));
console.log($.type([]));
</script>
```

## extend

> 用法：
>
> 1. 传一个参数，给jQuery函数对象添加属性，即扩充jquery的工具方法。
> 2. 传多个参数时，将后面的参数的属性合并到前一个参数中，属性名重复时更新属性值。
> 3. 传多个参数，但第一个参数为true时，使用`深拷贝`将后面参数合并到第一个对象中。

```html
<script src="../jquery-1.12.4.js" type="text/javascript"></script>
<script>

console.log('-------------------------extend------------------------------');
// extend(target, ...value); 传递多个参数时，将后面的类型中的内容合并到第一个参数中
var targetObj = { username: 'aaaa', age: 12 };
var obj1 = { address: '中国大山东', phone: '1313131' };
var obj2 = { username: 'Tom' };
console.log(targetObj, obj1, obj2);
// 将obj1和obj2中的属性合并到targetObj中，如果属性名相同，则更新。
var newObj = $.extend(targetObj, obj1, obj2);
console.log(newObj, targetObj, obj1, obj2, newObj === targetObj);

// extend()，传递一个参数时，给jQuery添加工具函数，即给核心函数对象添加属性。
$.extend({
    min: function (a, b) { return a > b ? b : a },
    max: function (a, b) { return a > b ? a : b }
});
console.log($.max(12, 13), $.min(12, 13));

// extend()，第一个参数为true时，表示对第二个参数进行深拷贝。
var targetObj1 = {};
var sourceObj1 = { p: { username: 'aaa', age: 12 }, className: '小红班' }
$.extend(targetObj1, sourceObj1);
console.log(targetObj1, sourceObj1, targetObj1.p === sourceObj1.p);

var targetObj2 = {};
var sourceObj2 = { p: { username: 'aaa', age: 12 }, className: '小红班' }
$.extend(true, targetObj2, sourceObj2);
console.log(targetObj2, sourceObj2, targetObj2.p === sourceObj2.p);
  
  
</script>
```

## toArray、contains

> toArray：将jQuery对象转换成真正的数组。
>
> contains：查询DOM对象是否包含另一个DOM对象。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>

    <div class="inner"> 111</div>
    <div class="inner"> 2222</div>
    <div class="inner"> 3333</div>
    <div class="inner"> 4444</div>
    <script src="../jquery-1.12.4.js" type="text/javascript"></script>
    <script>
        console.log('-------------------------toArray------------------------------');
      	// jquery是一个伪数组，其中保存着DOM对象元素，使用toArray将元素保存在一个真正的数组中。
        var arrs = $('.inner').toArray();
        console.log(arrs);
      
      console.log('-------------------------contains------------------------------');
      //参数介绍，第一个参数是否包含第二个参数，html元素节点是否包含body元素节点
      $.contains(document.documentElement, document.body); // true
			$.contains(document.body, document.documentElement); // false
    </script>
</body>

</html>

```

## parseJSON

> 将json字符串转成对象，传递null、undefined和空字符串返回null，json串格式不会时报异常。

```javascript
var obj = { p: { username: 'aaa', age: 12 }, className: '小红班', stu:['aaa', 'bbb', 'ccc']};

// 将对象转换成json字符串
var str = JSON.stringify(obj);
console.log(str);
// 将json字符串转换成对象
obj = $.parseJSON(str);
console.log(obj);
```

