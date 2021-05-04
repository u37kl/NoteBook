# Reflect

> ES6引入的反射机制，对象结构操作，在ES5中通过Object类对象进行操作，例如`给对象添加属性，Object.defineProperty`、`删除对象某个属性，delete target.name`、`获取对象属性，Object.getOwnPropertyNames`等，在ES6中单独使用一个`Reflect`类来实现这些操作，在未来会逐步淘汰Object类操作对象结构。

## set方法

> `Reflect.set(target, name, value, receiver)`
>
> * target：目标对象
> * name：给哪个属性赋值
> * value：属性值
> * receiver：接收者
>
> 使用方式：
>
> 1. 如果只想给target对象某个属性设置属性值或者添加某个属性，`Reflect.set()`方法只需要传递前3个参数。
> 2. 如果使用了第四个参数，则其实给receiver对象设置属性。

```javascript
var txx = {
    foo:1,
    set bar(value) {
        return this.foo = value;
      },
}
var rec = {
    baz:2
}
Reflect.set(txx, 'foo', 12, rec); // rec: { baz: 2, foo: 12 },相当于给rec对象添加了foo属性。
Reflect.set(txx, 'bar', 8, rec); // rec：{ baz: 2, foo: 8 }，相当于txx.bar.call(rec, 8)
```

```javascript
// receiver对象为proxy
var t = {
    foo:1
}
let prox = new Proxy(t, {
    set(target, key, value, receiver){
        console.log('set');
        Reflect.set(target, key, value, receiver);
    },
    defineProperty(target, key, attribute){
        console.log('defineProperty');
        Reflect.defineProperty(target, key, attribute);
    }
})

prox.a = 1;
// 触发prox对象的set拦截器，调用Reflect.set()方法，但是由于receiver对象是一个proxy，则触发proxy对象的defineProperty拦截器。
```



## get方法

> `Reflect.get(target, name, value, receiver)`
>
> * target：目标对象
> * name：给哪个属性赋值
> * value：属性值
> * receiver：接收者

```javascript
var ssw11 = {
    foo: 1,
    bar: 2,
    get baz() {
      return this.t1 + this.t2;
    },

    get b() {
        return this.foo + this.bar;
      },
};

var receive1 = {
    t1:12,
    t2:13,
}

let res = Reflect.get(ssw11, 'baz', receive1); // res.baz.call(receive1)
console.log(res);  // 25
let res1 = Reflect.get(ssw11, 'b');
console.log(res1); // 3
```

