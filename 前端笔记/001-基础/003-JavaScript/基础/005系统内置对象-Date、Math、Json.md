# Json对象

> JS系统提供了一个内置对象实现Json字符串到数组/实例对象之间的转换。
>
> 1. parse(字符串)，将字符串转换成对象或者数组。
> 2. stringify(数组/对象)，将数组或者对象转换成字符串。
>
> 注意：JSON对象不能转换实例对象中的方法，例如obj2转换成json字符串后，没有show属性。

```javascript
var jsonStr = '{"userName" : "aaa", "arr" : ["abc", 123, true, null, {"age" : 12}]}';
var obj = JSON.parse(jsonStr);
console.log(obj); // { userName: 'aaa', arr: [ 'abc', 123, true, null, { age: 12 } ] }


var obj1 = {userName : 'aaa', age : 12};
var str1 = JSON.stringify(obj1);
console.log(str1); // {"userName":"aaa","age":12}


var arr = ["abc", 123, true, null, {"age" : 12}];
var str2 = JSON.stringify(arr);
console.log(str2); // ["abc",123,true,null,{"age":12}]

var obj2 = {
    userName : 'aaa',
    show : function(){}
};
var str3 = JSON.stringify(obj2);
console.log(str3); // {"userName":"aaa"}
```



# Date对象

> JS内置对象，用来处理时间。
>
> 1. getFullYear()：获取当前年份
> 2. getMonth()：获取当前月份，从0开始算，因此要+1。
> 3. getDate()：获取当前天数。
> 4. getHours()：获取当前小时
> 5. getMinutes()：获取当前分钟
> 6. getSeconds()：获取当前秒
> 7. getMilliseconds：获取当前毫秒
>
> 设置日期：
>
> 1. setFullYear(year [, month, date])：设置当前年份、月份、天，月份和天可选
> 2. setMonth(month [, date])：设置当前月份、天，月份从0开始算，因此要+1，天可选
> 3. setDate(date)：设置当前天数。
> 4. setHours(hour [, minutes, seconds, milliseconds])：设置当前小时
> 5. setMinutes(minutes [, seconds, milliseconds])：设置当前分钟
> 6. setSeconds(seconds [, milliseconds])：设置当前秒
> 7. setMilliseconds：设置当前毫秒
>
> 日期与毫秒数转换：
>
> 1. getTime()：获取从1970-01-01 00:00:00开始到现在所走的毫秒数
> 2. setTIme()：将毫秒数转换成日期。
>
> 快速获取当前时间的毫秒数：
>
> 1. Date.now();

```javascript
var date = new Date();
console.log(date);
// 获取当前日期，注意月份从0开始，因此需要+1。
console.log(date.getFullYear(), date.getMonth()+1, date.getDate(), date.getHours(), date.getMinutes(), date.getSeconds(), date.getMilliseconds());

// 设置日期为2020-01-01，注意月份从0开始，因此需要-1。
date.setFullYear(2020, 0, 1);
console.log(date.getFullYear(), date.getMonth()+1, date.getDate(), date.getHours(), date.getMinutes(), date.getSeconds(), date.getMilliseconds());

// 设置日期为1-24日
date.setMonth(2,24);
date.setDate(25);
date.setHours(12, 45, 55, 111); 

// 获取从1970-01-01 00:00:00开始到现在所走的毫秒数，以number表示
var time = date.getTime();
console.log(typeof time, time); // number 1585111555111

// 从新设置时间，以毫秒数设置时间。
date.setTime(12);
console.log(date.getFullYear(), date.getMonth()+1, date.getDate(), date.getHours(), date.getMinutes(), date.getSeconds(), date.getMilliseconds());

console.log(Date.now());
```



# Math

> 取整函数：
>
> 1. floor()：向下取整
> 2. ceil()：向上取整
> 3. round()：四舍五入
>
> 三角函数：
>
> 1. sin()
> 2. cos()
> 3. asin()
> 4. acos()
> 5. tan()
> 6. atan()
>
> 绝对值：
>
> 1. abs()
>
> 最大最小值：
>
> 1. max()
> 2. min()
>
> 随机数：
>
> 1. random()，随机数获取区间为[0, 1)，左闭右开。

```javascript
var num1 = Math.floor(2.9);
var num2 = Math.ceil(2.1);
var num3 = Math.round(2.9);
var num4 = Math.round(2.1);
console.log(num1, num2, num3, num4); // 2 3 3 2


var arrs = [1,2,3,4,5,6];
// max和min接收可变参数，使用apply以数组形式接收
var max = Math.max.apply(null, arrs);
var min = Math.min.apply(null, arrs);
console.log(min, max);

var r1 = Math.random();
console.log(r1);

// 获取某个区间中的值。
function random(min, max){
   return  Math.floor(Math.random() * (max - min)) + min;
}

// 获取(20, 30)区间的任意值。
var r2 = random(20, 30);
console.log(r2);
```

