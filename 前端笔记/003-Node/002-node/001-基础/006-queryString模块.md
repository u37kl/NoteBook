# QueryString模块

> urlencode编码是啥？
>
> 1. get方式传递地参数就是urlencode方式编码，post默认情况下也是对参数进行urlencode方式编码，然后放在请求体中。
>
> QueryString模块作用？
>
> 1. 该模块最主要的功能就是处理urlencode编码的数据。

## 请求参数序列化和反序列化

> 函数说明：
>
> 1. stringify(obj, separator, eq, option)：将对象转成application/x-www-form-urlencoded编码方式。
>
> 2. parse(obj, separator, eq, option)：将application/x-www-form-urlencoded编码方式的字符串转成对象。
>
>  参数说明：
>
> 1. separator：键值对分隔符，默认为&
> 2. eq：键值之间的分隔符，默认为=
> 3. option：配置参数，maxkeys参数为解码或者编码几个键值对。
>
> 

### 请求参数反序列化

```javascript
querystring.stringify({name: 'whitemu', sex: [ 'man', 'women' ] });
/*
return:
'name=whitemu&sex=man&sex=women'
*/

querystring.stringify({name: 'whitemu', sex: [ 'man', 'women' ] },"*","$");
/*
return:
'name$whitemu*sex$man*sex$women'
*/
```



### 请求参数序列化

```javascript
querystring.parse("name=whitemu&sex=man&sex=women");
/*
return:
{ name: 'whitemu', sex: [ 'man', 'women' ] }
*/
querystring.parse("name=whitemu#sex=man#sex=women","#",null,{maxKeys:2});
/*
return:
{ name: 'whitemu', sex: 'man' }
*/
```



## urlencode编码

> ​		urlencode编码在网络请求中非常常见，例如url地址的查询参数中出现了非ASCII码时，就会对url进行urlencode编码，就是将数据按照单字节转换成ASCII码表示。这是历史原因所造成的，设计url时使用的是ASCII码，url中存在一些不安全的字符，为了兼容其它编码格式，才出现了urlEncode编解码。
>
>  
>
> 当查询字符串中出现不安全字符串时，会将其转换成`%ASCII码`
>
> 1. http://localhost:8080/?name=中国&password=小镇  
> 2. http://localhost:8080/?name=%E4%B8%AD%E5%9B%BD&password=%E5%B0%8F%E9%95%87



### 编码

```javascript
querystring.escape("name=慕白");
/*
return:
'name%3D%E6%85%95%E7%99%BD'
*/
```



### 解码

```javascript
querystring.unescape('name%3D%E6%85%95%E7%99%BD');
/*
return:
'name=慕白'
*/
```

