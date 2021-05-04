# delete命令

> javascript中，对于那些不需要的属性，如果设为null或者undefined，属性保存在对象中，通过`delete命令`可以实现删除对象中的某个不用的属性。

```javascript
var obj = {
  a:{name:'adb'},
  b:[1,2,3,4]
};

delete obj.b; // 删除obj中b属性。
let t = 'a';
delete obj[t]; // 通过变量删除obj中的属性
```

> **delete使用注意**：
>
> 1. delete只能删除对象的属性，不能删除对象，如果想要删除一个对象，前提是这个对象是某个对象的属性。
> 2. delete只能删除对象本身的属性，不能删除原型链上的属性。
> 3. delete不能删除不可配置的属性，即如果该属性的属性描述`configuration = true`，则无法删除该属性。
> 4. delete无法删除全局作用域中定义的函数。
> 5. 不能通过删除let、const、var定义的变量。这些变量通过作用域销毁由系统删除。如果想要删除全局作用域中的变量，那么不要用var，使用window.变量名创建变量，因为在全局作用域中使用var定义的变量最终保存在window中。