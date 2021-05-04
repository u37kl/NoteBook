# 快速生成html标签

> Html有两种简写方式，Emmet和Haml，下面使用的是Emmet写法，通过书写一些简单的字符串，然后点击`tab键`让webstorm生成完成的html标签。
>
> 
>
> Emmet写法：
>
> 1. E 代表HTML标签。
> 2. E#id 代表id属性。
> 3. E.class 代表class属性。
> 4. E[attr=foo] 代表某一个特定属性。
> 5. E{foo} 代表标签包含的内容是foo。
> 6. E>N 代表N是E的子元素。
> 7. E+N 代表N是E的同级元素。
> 8. E^N 代表N是E的上级元素。(不理解)
> 9. E\*n 代表连续创建n个并列的E标签。
> 10. E$\*n 代表连续创建n个并列的E标签，并且和给标签属性编号，`$表示给标签中的某个属性编号，号码为一位，$$表示号码为2位`

```html
<!-- div -->
<div></div>

<!-- div#divname -->
<div id="divname"></div>
<!-- div.classname -->
<div class="classnam"></div>
<!-- div[width=400] -->
<div width="400"></div>
<!-- div{这是一个标签} -->
<div>这是一个标签</div>

<!-- div>p -->
<div>
    <p></p>
</div>
<!-- div.class1>p.class2{p标签内容} -->
<div class="class1">
    <p class="class2">p标签内容</p>
</div>

<!-- div+p+p+p 生成同级标签 -->
<div></div><p></p><p></p><p></p>
<!-- div^p -->
<div></div><p></p>

<!-- div.class1*3 -->
<div class="class1"></div>
<div class="class1"></div>
<div class="class1"></div>

<!-- div.class$*3 -->
<div class="class1"></div>
<div class="class2"></div>
<div class="class3"></div>

<!-- div#item$.class$&*3  -->
<div id="item1" class="class1"></div>
<div id="item2" class="class2"></div>
<div id="item3" class="class3"></div>

<!-- div#ids${我是div$}*3>a.cl1[href=#]{我是a标签$}*2 -->
<div id="ids1">我是div1<a href="#" class="cl1">我是a标签1</a><a href="#" class="cl1">我是a标签2</a></div>
<div id="ids2">我是div2<a href="#" class="cl1">我是a标签1</a><a href="#" class="cl1">我是a标签2</a></div>
<div id="ids3">我是div3<a href="#" class="cl1">我是a标签1</a><a href="#" class="cl1">我是a标签2</a></div>
 
<!-- table>((thead>tr>th*5)+(tbody>tr>td*5)) -->
<table>
    <thead>
    <tr>
        <th></th>
        <th></th>
        <th></th>
        <th></th>
        <th></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    </tbody>
</table>
```
