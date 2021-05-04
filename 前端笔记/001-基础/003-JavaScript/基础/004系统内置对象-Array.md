# JS内置对象-Array



## Array创建

>JS提供了三种创建方式：
>
>1. 字面量
>2. 构造函数
>3. of方法(ES6新增方法)

```javascript
// 使用字面量创建
var a = [1, 2, 'a', true];

// 使用构造函数创建，参数是1个时，将其看成数组长度，参数多个时，将其看成数组的元素，因此数组b为长度为3的空数组，数组c是[1, 2, 'a', true]。
var b = new Array(3);
var c = new Array(1, 2, 'a', true);

// 将参入的参数以数组形式保存。
var d = Array.of(1, 2, 'a');
```



## Array特点

> JS数组特点：
>
> 1. JS数组是可变数组，因此给越界位置赋值、修改数组的length属性，都可以改变数组的长度。
> 2. JS数组中可以保存多种类型的数据，不像其它语言，数组中只能保存一种类型数据。

```javascript
var a = [1];

// 给越界位置赋值时，会修改数组长度。
a[3] = 2; // [1, undefined, undefined, 2]

// 修改length属性，会修改数组长度
a.length = 1; // [1]
```



## Array中的方法

>方法注意的三要素：方法的功能、方法的返回值、是否影响原数组。
>
>
>
>Array.concat()：，连接数组，会返回一个新数组，如果不传任何参数时为对原数组的深拷贝。
>
>Array.join()：将数组以指定的分隔符，转换成字符串形式输出
>
>Array.length();： 返回数组长度。
>
>Array.pop(); ：将数组尾部元素移出数组，并返回该元素，修改的是当前数组。
>
>Array.push();：向数组尾部添加一个元素，修改的是当前数组。
>
>Array.shift();：将数组头部元素移出数组，并返回该元素，修改的是当前数组。
>
>Array.unshiflt();：数组头部添加一个元素，修改的是当前数组。
>
>Array.sort();：对数组进行排序，传入一个比较函数。返回排序后的数组。
>
>Array.reverse()：将数组中的元素颠倒。
>
>Array.slice();：截取一部分连续空间的元素，生成新数组。
>
>Array.splice();：替换数组中的元素。
>
>Array.toString();：将数组转换成字符串。
>
>Array.toLocalString();：按照本地语言将数组转换成字符串。

```javascript
/**
* join()、toString()、toLocalString()
* toString()和toLocalString()，会调用数组中每个元素的toString和toLocalString方法，将其拼成一个字符串返回。
* join()，数组会调用每个元素的toString方法，将其拼接成一个字符串返回。
*/ 

// 定义Person引用类型
   function Person(name){
       this.name = name;
       this.valueOf = function(){
           return this.name;
       }
   }

// 重写Person引用类型的toString方法
   Person.prototype.toString = function(){
       return 'bbb';
   };
// 重写Person引用类型的toLocaleString方法
   Person.prototype.toLocaleString = function(){
       return 'ccc';
   };

   var p1 = new Person('aaa');
   var p2 = new Person('sss');
   var arrm2 = [p1, p2, null, undefined];

   console.log(arrm2.valueOf());// [Person{}, Person{}, null, undefined]
   console.log(arrm2.toString());// bbb,bbb,,
   console.log(arrm2.toLocaleString());// ccc,ccc,,
   console.log(arrm2.join('|')); // bbb|bbb||

```

```javascript
/**
* concat()
* concat方法会对原数组进行深拷贝，并将参数中的元素，添加到新数组中，即使参数是数组，也是将其中的元素取出并保存
*/
var arr1 = [1, 2, null, 'aaa'];
var arr2 = [4, 5, 6];
var arr3 = arr2.concat('bbb', true, arr1); // [4,5,6,'bbb',true,1,2,null,'aaa']

```

```javascript
/**
* push()、pop()、shift()、unshift()，push和unshift返回数组长度，pop和shift返回刚刚删除的元素值。
* push()和pop()，将数组尾部作为栈顶。
* shift()和unshift()，将数组首部作为栈顶。
*/

var arr1 = [1, 2, null, 'aaa'];
arr1.push('bbb'); // [1, 2, null, 'aaa', 'bbb']
arr1.pop(); // [1, 2, null, 'aaa']
arr1.unshift('bbb'); // ['bbb', 1, 2, null, 'aaa']
arr1.shift(); // [1, 2, null, 'aaa']
```

```javascript
/**
* sort()和reverse()
* 两个函数都是在原数组中修改元素的位置，排序完毕后返回当前数组。
*/

var arr1 = [1, 2, 5, 4, 3];
var arr2 = arr1.reverse(); // [3, 4, 5, 2, 1]
var arr3 = arr1.sort(function(value1, value2){
  return value1 <= value2 ? -1 : 1;
}); // [1, 2, 3, 4, 5]
```

```javascript
/**
* slice()，截取子数组，返回一个新数组，不会影响原数组
* 1. 传入一个参数时，从该参数的位置到数组末尾的元素拷贝到新数组中。
* 2. 传入两个参数时，从第一个参数位置到第二个参数位置前一个位置的元素拷贝到新数组中。
* 3. 传入参数为负数时，函数会使用数组长度+参数获得位置。
* 4. 传入的参数第一个参数 > 第二个参数时，参数不合法返回空数组。
*/

var arr1 = [1, 2, 3, 4];
var newarr1 = arr1.slice(0, 2); // [1, 2]
```

```javascript
/**
* splice()，替换数组，直接在原数组上修改，如果替换操作存在删除元素情况，将删除的元素保存在新数组中返回。
* 1. 只添加时，返回空数组。
*
* 参数1: 位置
*  参数2: 长度，长度为0时，表示只插入元素，不删除和替换
*  参数3: 需要插入或者替换的多个元素。
*/

var arr1 = [1, 2, 3, 4, 5];
// 删除操作
var newArr1 = arr1.splice(0, 2); // [0, 1]
console.log(arr1); // [3, 4, 5]

// 插入操作
var newArr2 = arr1.splice(1, 0, 'a', 'b'); // []
console.log(arr1); // [3, 'a', 'b', 4 ,5]

// 替换操作
var newArr3 = arr1.splice(0, 1, 'c', 'd'); // [3]
console.log(arr1); // ['c', 'd', 'a', 'b', 4, 5]
```



## ES5 新增方法

> Array.prototype.indexOf()：查询某个字符串位置，从前往后查
> Array.prototype.lastIndexOf()：查询某个字符串位置，从后往前查
> Array.prototype.forEach()：遍历数组
> Array.prototype.map()：对数组中所有元素进行操作，生成一个新数组， 不会检查空数组，不会改变原数组
> Array.prototype.filter()：提取符合条件的元素组成一个新数组。
> Array,prototype.every()：检查数组中所有元素是否都满足某个条件
> Array.prototype.some()：检查数组中是否存在满足某个条件的元素。

```javascript
var arr1 = [1, 2, 3, 4, 5, 4 ,null , undefined];
   
// 让数组中所有元素+4
var newArr1 = arr1.map(function(value, index, arr){	return value + 4 ; });
 console.log(newArr1);

// 提取出数组中大于4的元素
var newArr2 = arr1.filter(function(value, index, arr){	return value > 4 ; });
 console.log(newArr2);


// 判断数组中是否都可以转换成数值
var isTrue1 = arr1.every(function(value, index, arr){  return value != NaN;  });
console.log(isTrue1);

// 判断数组中是否存在大于4的元素
var isTrue2 = arr1.some(function(value, index, arr){  return value > 4; });
console.log(isTrue2);
```



## ES6 新增方法

> Array.from()：将伪数组和可遍历对象转换为真正对象
>
> Array.of()：将方法中接收到的参数保存到新数组中。
>
> Array.find()：找出第一个满足条件的元素
>
> Array.findIndex()：找出第一个满足条件的元素所在数组的位置

```javascript
// obj就是一个伪数组，即属性名是一个数字，模仿数组的下标。
var obj = {
  0 : 'aaa',
  1 : 'bbb',
  2 : 'ccc',
  3 : 'ddd',
  length : 4
};

var arr = Array.from(obj); // 将伪数组转成真数组。
```



## 伪数组

> 伪数组，不是真正的数组，而是由自定义的对象形成的一种形似数组的对象。与数组最大的区别在于原型链中没有`Array.prototype`.
>
> 
>
> 伪数组条件：
>
> 1. 属性名用连续的数字，从0开始。
> 2. 需要有length属性，统计数字属性名个数。
> 3. 原型链中没有Array.prototype
>
> JS中最常见的伪数组：函数中的arguments和DOM中的childNodes

```javascript
// obj就是一个伪数组，即属性名是一个数字，模仿数组的下标。
var obj = {
  0 : 'aaa',
  1 : 'bbb',
  2 : 'ccc',
  3 : 'ddd',
  length : 4
};
console.log(obj[2]); // 以数组方式访问，中括号加下标。
```

