# Object中方法



## Object.key()

>  Object.key(targetObj)获取参数中对象的所有可枚举(enumable:true)的属性名，组成一个数组返回。`方法参数必须是对象`。

## Object.values()

> Object.values(targetObj)：获取参数中对象的所有可枚举(enumable:true)的属性值，组成一个数组返回。`方法参数必须是对象`。

## Object.getOwnPropertyNames()

> 与Object.key()方法相比，Object.getOwnPropertyNames()可以获得对象自身所有的属性名，包括不可枚举的属性，但不包括Symbol值作为名称的属性。`返回一个由属性名组成的字符串数组`。



## Object.defineProperty()

>该方法用来给对象添加属性(存储属性和计算型属性)。
>
>参数列表：
>
>1. 目标对象
>2. 对象的属性名
>3. 属性描述对象{configuration,enumable,writable,value}
>
>返回值：
>
>1. 返回添加属性的这个对象，也就是参数列表的第一个参数
>
>方法使用问题：
>
>1. 使用该方式添加的属性的继承性，js通过在`SuperObject.call(this)`实现继承，因此对于计算型属性，原本就是一对getter和setter方法，因此会将这两个方法添加到子类实例对象中。
>2. 当使用该方法多次设置同对象同属性名，其实是覆盖操作，前提是属性的configuration为true，因此可以将属性从存储属性转成计算型属性，再从计算型属性转成存储属性。

```javascript
var o = {};

o.a = 1;
// 等同于：
Object.defineProperty(o, "a", {
  value: 1,
  writable: true,
  configurable: true,
  enumerable: true
});


// 另一方面，
Object.defineProperty(o, "a", { value : 1 });
// 等同于：
Object.defineProperty(o, "a", {
  value: 1,
  writable: false,
  configurable: false,
  enumerable: false
});
```



## Object.assign()

> 该方法功能：从第2个参数开始，依次将参数对象中的属性合并到第一个参数对象中。
>
> 参数列表：
>
> 1. 目标对象
> 2. 源对象，它是可变参数，可以传入多个源对象，
>
> 返回值：
>
> 1. 将参数列表中第一个参数返回。
>
> 方法使用：
>
> 1. 它是从第2个参数对象开始，依次取出对象中的属性添加到第一个参数对象中，如果第一个参数对象已经有了该属性，则覆盖之前的属性值。
> 2. 可以使用该方法实现对某个对象的克隆，但是这个方法是`浅拷贝`。
> 3. 在拷贝过程中，出现异常时会中断拷贝，导致后面没有参数无法合并到该对象中。
> 4. 该方法只能拷贝源对象中自身的可枚举的属性，对于`不可枚举`、`父类属性`、`继承属性`无法拷贝。
> 5. 对于计算型属性，将它的getter和setter方法拷贝到target对象中。

```javascript
const o1 = { a: 1, b: 1, c: 1 };
const o2 = { b: 2, c: 2 };
const o3 = { c: 3 };

const obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }

// ---------------------拷贝时出现异常-----------------------------------------------
const target = Object.defineProperty({}, "foo", {
    value: 1,
    writable: false
}); // target 的 foo 属性是个只读属性。

Object.assign(target, {bar: 2}, {foo2: 3, foo: 3, foo3: 3}, {baz: 4});
// TypeError: "foo" is read-only
// 注意这个异常是在拷贝第二个源对象的第二个属性时发生的。

console.log(target.bar);  // 2，说明第一个源对象拷贝成功了。
console.log(target.foo2); // 3，说明第二个源对象的第一个属性也拷贝成功了。
console.log(target.foo);  // 1，只读属性不能被覆盖，所以第二个源对象的第二个属性拷贝失败了。
console.log(target.foo3); // undefined，异常之后 assign 方法就退出了，第三个属性是不会被拷贝到的。
console.log(target.baz);  // undefined，第三个源对象更是不会被拷贝到的。
```



## Object.create()

> 该方法用来根据一个原型对象创建一个新对象。该方法常用来实现继承
>
> 参数列表：
>
> 1. 创建完成的新对象的原型对象
> 2. 属性描述对象，可选
>
> 返回值：
>
> 1. 返回一个新对象

```javascript
function Person(){}

let t = Object.create(Person.prototype, { // 对象t的原型对象为Person.prototype
  // foo会成为所创建对象的数据属性
  foo: {
    writable:true,
    configurable:true,
    value: "hello"
  },
  // bar会成为所创建对象的访问器属性
  bar: {
    configurable: false,
    get: function() { return 10 },
    set: function(value) {
      console.log("Setting `o.bar` to", value);
    }
  }
});
```



## Object.is()

> 方法用来判断两个值是否相等。
>
> 方法使用：
>
> 1. 方法与`===`一样，不会进行类型转换。
> 2. 都等于+0
> 3. 都等于-0
> 4. NaN == NaN
> 5. null == null
> 6. undefined == undefined
>
> 在`===`和`==`这两个运算符眼里，`NaN != NaN`，`+0 == -0`

```javascript
let isTrue = Object.is(NaN, NaN);
let isTrue = Object.is({}, {})

// polyfill
function is(left, right){
  if(left === right){
    return left!==0 || 1/left === 1/right; // 将+0与-0视为不等。
  }else{
    return left !== left && right !== right; // 验证left和right是否为NaN。
  }
}
```

