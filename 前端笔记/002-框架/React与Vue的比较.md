# 用法比较

## 加载资源上

> React需要通过import将组件需要使用的css文件、img文件进入到组件中，然后在render函数中模版html代码中使用。
>
> ```
> import from './css/loin.css';
> import logo from './img/logo.png'
> <img class="haha" src={logo}>
> ```
>
> 
>
> vue中与react一样，在<script>标签中`import 'swiper/css/swiper.css'`对于图片不需要引入直接通过地址访问。
>
> ```html
> <template>
> 	<img class="haha" src='./img/logo.png'>
> </template>
> <script>
> 		import 'swiper/css/swiper.css' // 第三方css
> </script>
> <style scoped>
> 		@import from './css/loin.css'; // 这个并不是webpack引入文件，而是css模块化，将一些mixin函数、函数封装到一个css文件，供其他css文件使用。
> </style>
> ```
>
> 
>
> 