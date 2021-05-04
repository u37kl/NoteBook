# Iterator

> ES6提供的遍历统一接口，只有实现该接口的对象都可以使用统一的方式进行遍历获取其中的资源。
>
> 1. for...of
> 2. ...obj

## Iterator接口

```javascript
interface Iterable{
		[Symbol.iterator]: Iterator
}

interface Iterator{
  // 获取下一个元素
  next: function(){
    return IterationResult;
  },
  // 中断遍历时使用
  return: function(){
    return IteratorResult;
  }
}

interface IterationResult{
  value: any,
  done: boolean
}
```

```javascript
// 对象实现iterator接口
function Person(){
    this.name = "aaa";
    this. age = 18;
    this.address = 'sdwd';
  	// 实现Iterable接口，此时Person类的实例对象就可遍历了。
    this[Symbol.iterator] = function(){
      let index = 0;
      let keys = Reflect.ownKeys(this);
      
      		// 实现Iterator接口
          return {
            // 获取下一个元素
            next: ()=>{
            if(index < keys.length){
              // 实现IterationResult接口
              return {value:this[keys[index++]], done:false}
            }else{
              return {done:true}
            }
          },
          // 该方法用来实现遍历中断的操作，例如在遍历时使用return、break、或者遍历时出现了异常
          return: ()=>{
            // .... 做一些收尾操作，例如关闭IO连接，
            return {done:true}
          }
      }
    }
}

let pp = new Person();
for (const iterator of pp) {
    console.log(iterator); // aaa 18 sdwd [Function]
}

for (const iterator of pp) {
    console.log(iterator); // aaa 18 sdwd [Function]
  	if(iterator == '[Functon]'){
      break; // 会调用iterator接口的return方法
    }
}
let a = [...pp] // [ 'aaa', 18, 'sdwd', [Function] ]
```



## 伪数组遍历

```javascript
let fakeArr = {
  0: 'a',
  1: 'b',
  2: 'c',
  3: 'd',
  length: 4
}
// 给伪数组添加Array原生的的iterator接口实现，这样可以实现伪数组的遍历。
fakeArr[Symbol.iterator] = Array.prototype.[Symbol.iterator];
for(const iterator of fakeArr){
  console.log(iterator)
}
```



## ES6中本身支持iterable接口的数据类型

> 1. string
> 2. Array
> 3. Set
> 4. Map
>
> Array类型使用for...of只能获取value值，如果想要获取key，通过`entries`或者`keys`方法。

```javascript
// string基本类型会转换成String对象类型，String类本身就实现了Iterable接口
for (const iterator of 'abcdfertgh') {
    console.log('222-',iterator);
} 

// Set类型
let set = new Set().add('aaa').add('bbb').add('cccc');
for (const iterator of set) {
    console.log(iterator)
}

// Map类型
let map = new Map().set('a',11).set('b',22).set('c',33);
for (const [key, value] of map) {
   console.log(key, value); 
}

// 数组类型
let arr = [1,2,3,4,5];
for (const iterator of arr) {
    console.log(iterator); // 1,2,3,4,5
}
for (const iterator of arr.entries()) {
    console.log(iterator); // [0,1],[1,2],[2,3],[3,4],[4,5]
}

for (const iterator of arr.keys()) {
    console.log(iterator, arr[iterator]); // 0 1, 1 2, 2 3, 3 4, 4 5,
}
```



## 数组遍历方式比较

> for--index
>
> for-each
>
> for---in
>
> for---of

```javascript
let arr = [1,2,3,4,5];
arr.a = 'aa'

// 传统方式，麻烦
for(let index; index < arr.length; index++){
  console.log(index, arr[index])
}

// for-each，好处是不会遍历出非数组元素的属性，缺点是中断遍历，即break、return关键字无效
arr.forEach((e, index) => {
    console.log(e, index);
});

// for-in，好处可以中断遍历，但是会遍历出非数组元素的属性，例如数组中的a属性
for (const key in arr) {
    if (Object.hasOwnProperty.call(arr, key)) {
        const element = arr[key];
        console.log(element);
    }
}

// for--of，可以中断遍历、不会遍历出非数组元素属性，并且编写简单
for(const iterator of arr){
  console.log(iterator)
}
```

