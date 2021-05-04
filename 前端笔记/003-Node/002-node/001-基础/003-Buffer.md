# Buffer

> ​		node.js提供的一种类似数组的数据类型，JS是没有处理二进制数据的数据类型，node.js由于可以做服务器开发，Buffer的诞生就是为了处理磁盘上的音视频、图片这些二进制数据。
>
>  
>
> Buffer特点：
>
> 1. 对于字符串是以utf8编码格式表示。
> 2. 一旦创建了buffer，它的大小就是固定的，不能在改变。
> 3. buffer是一个类数组的结构，其中每个元素大小为`1个字节`。

## Buffer使用方式

> Buffer创建方式：
>
> 1. new Buffer(size)、new Buffer(str)
> 2. Buffer.alloc(size, fill)
> 3. Buffer.allocUnsafe(size)

```javascript
/**
 *  new Buffer()创建Buffer实例对象，已经被废弃了。
 */
// 默认使用utf8对字符串进行编码，汉语用三个字节表示
let bf0 = new Buffer('中国')
// 接收长度的单位为字节，1表示1B；
let bf1 = new Buffer(1);
bf1.write('abc'); // <Buffer e4 b8 ad e5 9b bd>
console.log(bf0, bf1); // <Buffer 61>
 
// 申请一块新空间，第二个参数不传或者传undefined，直接初始化为00。
let bfa1 = Buffer.alloc(5, undefined); // <Buffer 00 00 00 00 00>
let bfa2 = Buffer.alloc(5, 'a'); // <Buffer 61 61 61 61 61>
console.log(bfa1, bfa2);
 
// 该函数创建Buffer的速度非常快，不抹除内存空间中遗留的垃圾数据，使用时要覆盖所有字节，否则污染新数据。
let bfau1 = Buffer.allocUnsafe(5); // <Buffer d8 24 81 04 01>
console.log(bfau1);
```



### 将其它类型转换成Buffer

> Buffer.from()函数，该方法转换的数据类型：
>
> 1. 字符串
> 2. 数组
> 3. 伪数组
> 4. 实现valueOf函数的实例对象
>
> 该函数可以将上面常见四种类型转换成二进制

```javascript
function Person(){
    this.valueOf = function(){
        return '1111';
    }
}

// 将字符串、数组、伪数组、实现valueOf函数的实例对象，转换成二进制数据。
let bff1 = Buffer.from('abc'); // <Buffer 61 62 63>
let bff2 = Buffer.from([1,2,3,4]); // <Buffer 01 02 03 04>
let bff3 = Buffer.from(new Person()); //  <Buffer 31 31 31 31>
console.log(bff1, bff2, bff3);
```



### 将数据写入到一个空Buffer

> write()函数，将数据写入到buffer中，`如果写入内容超过buffer大小，则舍弃超过的部分`。

```javascript
/*
* 将文本写入到buffer中，实例对象函数write和类对象函数from
* 输入的数据超过buffer大小，则舍弃超过的部分。
*/
let buff = Buffer.alloc(8);
buff.write('123456789');
// 将buffer转成字符串
let str = buff.toString('utf8',0, buff.length);
console.log(buff, str);
```



### json与buffer的数据转换

> toJSON()：将Buffer数据转换成json，该方法转换的前提是，Buffer保存的是字符串、数组、实现valueOf方法的实例对象

```javascript
// buffer转成json
let jsonBuf = Buffer.from([1,2,3,4,5]);
console.log(jsonBuf.toJSON()); // { type: 'Buffer', data: [ 1, 2, 3, 4, 5 ] }
 
// 将json转换成Buffer
var jsonBuf2 = JSON.parse(json, (key, value)=>{
   return value && value.type == 'Buffer' ? Buffer.from(value.data) : value;
});
console.log(jsonBuf2); //  <Buffer 01 02 03 04 05>
```



## Buffer保存字符串的乱码问题

> 例如：使用流式读取文件中的文本时，读取函数会返回Buffer对象，进行数据合并时使用`+`合并，就容易造成乱码的问题，像下面的例子读取一首诗，Buffer大小为11B，一个汉字占3B，第一次次读取读到3个半字，使用`+`合并时会转换成字符串，最后半个字就会变成乱码，此时后面的数据已经成为乱码了。

```javascript
// 使用+方式间接Buffer读取的内容，最后读取出来的就是中文+乱码。
let buff1 = Buffer.from('床前明月光,疑是地上霜,举头望明月,低头思故乡');
let data = '';
let buff2 = Buffer.alloc(11);
for (let i = 0; i <= buff1.length; i+=11) {
    buff1.copy(buff2,0, i, i+10);
    data += buff2;
}
console.log('是：'+data); // 是：床前明��光,疑�地上霜,��头望�月,低头��故乡��

// 使用数组接Buffer，然后使用concat合并
let arr = [];
buff2.fill(0);
for (let i = 0; i <= buff1.length; i+=11) {
    buff1.copy(buff2,0, i, i+11);
    arr.push(Buffer.from(buff2));
}
let str1 = Buffer.concat(arr,63).toString()
console.log(str1); // 床前明月光,疑是地上霜,举头望明月,低头思故乡
```

