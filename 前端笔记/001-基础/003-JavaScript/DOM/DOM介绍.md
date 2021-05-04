# DOM

> DOM技术：文档对象模型，针对html和XML文档的一个API，实现对网页的结构、内容进行更新。它是一个跨平台的技术。
>
> 由于html和XML文档的树形结构特点，浏览器会生成一个层次化节点树来表示当前文档，开发者通过操作文档树来更新网页。
>
> 是
>
> 文档树的根节点：document对象，所有操作都是基于这个对象开始的。

## DOM节点

> DOM将html和XML文档中的元素解析成一个个节点，一共分为了12种类型的节点，它们都继承自`Node类型`，其中最重要和最常用的是四种节点，分别为：
>
> 1. 元素节点：html和XML文档中的标签元素。
> 2. 文本节点：html和XML文档中的文本内容。
> 3. 属性节点：标签中的属性。
> 4. 注释节点，html和XML文档中的注释，不太常用。

### 几种种主要的DOM节点

> DOM提供了节点属性描述节点：
>
> 1. nodeName：节点名称
> 2. nodeType：节点类型
> 3. nodeValue：节点值
>
> |    节点    |  nodeName   | nodeType | nodeValue |
> | :--------: | :---------: | :------: | :-------: |
> |  元素节点  | 标签名称(P) |    1     |   null    |
> |  文本节点  |    #text    |    3     | 文本内容  |
> |  属性节点  |   属性名    |    2     |  属性值   |
> |  注释节点  |  #comment   |    8     | 注释内容  |
> | 文档根节点 |  #document  |    9     |   null    |

```html
<!DOCTYPE html>
<html>
    <body>
        <!-- 注释-->
        <input type="text" name="ssv" value="aaaa" />
        <p value="asdasd">文本111</p>
        <script>
          	<!-- 文档对象节点-->
            console.log(document.nodeName, document.nodeType, document.nodeValue)
          
	          <!-- 元素节点-->
            var pNode = document.getElementsByTagName("p")[0];
            console.log(pNode.nodeName, pNode.nodeType, pNode.nodeValue)
          	
          	<!-- 元素节点-->
            var inputNode = document.getElementsByTagName("input")[0];
            console.log(inputNode.nodeName, inputNode.nodeType, inputNode.nodeValue)

 		        <!-- 属性节点-->
            var attributeNode = inputNode.getAttributeNode('name');
            console.log(attributeNode.nodeName, attributeNode.nodeType, attributeNode.nodeValue)
          
          	<!-- 文本节点-->
            var textNode = pNode.firstChild;
            console.log(textNode.nodeName, textNode.nodeType, textNode.nodeValue)
          
	          <!-- 注释节点-->
            var commentNode = document.body.childNodes[1];
            console.log(commentNode.nodeName, commentNode.nodeType, commentNode.nodeValue)
        </script>
    </body>
</html>
```



## DOM文档树

> 浏览器解析html或者XML文档时，会将其解析成树形数据结构(文档树)，文档树中包含着该文档所有的内容。
>
> Node类型提供了几种属性来描述节点的关系：
>
> 1. childNodes：当前节点的所有子节点
> 2. children：当前节点的所有元素节点，低版本浏览器中为当前节点的所有元素节点和注释节点
> 3. parentNode：当前节点的父节点
> 4. parentElement：当前节点的父元素节点，所有浏览器都支持
> 5. firstChild：当前节点的第一个子节点
> 6. lastChild：当前节点的最后一个子节点
> 7. previousSibling：当前节点的前一个兄弟节点
> 8. nextSibling：当前节点的后一个兄弟节点
> 9. firstElementChild：当前节点的第一个元素节点
> 10. lastElementChild：当前节点的最后一个元素节点
> 11. previousELementSibling：当前节点的前一个兄弟元素节点
> 12. nextElementSibling：当前节点的后一个兄弟元素节点
>
> 高版本浏览器才有的属性：
>
> 1. firstElementChild
> 2. lastElementChild
> 3. previousELementSibling
> 4. nextElementSibling
>
> 上面说的获取节点和获取元素节点是不同的，由于代码格式化的原因，父子标签之间、兄弟标签之间都会有空格换行情况，空格换行是 文本节点。
>
> 
>
> 下面的例子来解释子节点和子元素节点：

```html
<html>
  <body>
    <ul>
       sss
       <li> aaa </li>
       <li id="liTest"> bbb </li>
       <li> ccc </li>
       dddd
    </ul>
  </body>
  
  <script>
  		window.load = function(){
       	var ulNode = document.getElementsByTagName('ul')[0];
    	  var nodeList1 = ulNode.childNodes; // 7个节点，文本节点4个，元素节点3个，li标签之间的空格换行就是文本节点
    	  var nodeList2 = ulNode.children; // 3个节点，只包含元素节点，低版本浏览器会包含注释和元素节点
    	  var firstSonNode = ulNode.firstChild; // 第一个子节点，为字符串“        sss       ”
        var lastSonNode = ulNode.lastChild; // 最后一个子节点，为字符串“        dddd       ”
    	
    	  var liNode = document.getElementById('liTest');
    	  var liParent = liNode.parentNode; // 获取当前节点的父节点，当前节点父节点为 ul标签
    	  var liParent1 = liNode.parentElement; // 获取当前节点的父元素节点，与parentNode的是返回的父节点是元素节点，由于只有元素节点才能有子节点因此两者相同。
    	  var previousNode = liNode.previousSibling; // 当前节点的前一个节点，为字符串“           ”
        var nextNode = liNode.nextSibling; // 当前节点的后一个节点，为字符串“         ”
      
    	  <!-- 下面是只有高版本浏览器才支持的 -->
        var firstSonElementNode = ulNode.firstElementChild; // 第一个子节点，为<li>aaa</li>
        var lastSonElementNode = ulNode.lastElementChild; // 最后一个子节点，为<li>ccc</li>
    		var previousElementNode = liNode.previousElementSibling; // <li>aaa</li>
     	  var nextElementNode = liNode.nextElementSibling; // <li>ccc</li>
      }
  </script>
</html>

```

```javascript
// 由于IE8及以下版本浏览器不支持firstElementChild这些属性，因此使用兼容性写法。
function(node){
  if(node.firstElementChild){
    return node.firstElementChild;
  }else{
    var temp = node.firstChild;
    while(temp && temp.nodeType != 1){
      temp = temp.nextSibling;
    }
    return temp;
  }
}
```



## DOM节点操作

> DOM节点操作：包括对节点增删改查情况，不同类型节点，DOM提供了不同的方法进行操作。
>
>  
>
> `重点`：所有DOM操作代码必须等待DOM树加载完成才能执行，因为当在JS引擎在生成DOM树时，操作DOM节点时有可能操作的节点js引擎还没有创建完成。因此所有DOM操作代码只有写在两种地方：
>
> 1. window.onload代码中。
> 2. body标签的地步，让包含DOM操作的script作为body标签的最后子节点。

### 节点的创建

```javascript
 // 元素节点的创建
var elementNode = document.createElement('标签名称');
elementNode.id = 'idN';
elementNode.className = "clsN";
elementNode.style.border = '5px 5px'; // 设置css样式

// 属性节点的创建
var attrNode = document.createAttribute('参数作为属性节点的名称');
attrName.nodeValue = 'attrValue';

// 文本节点的创建
var textNode = document.createTextNode("参数作为文本节点的内容")
 // 注释节点的创建
var conmentNode = document.createComment('参数作为注释节点的内容');
```



### 文本节点的操作

> 文本节点不会单独存在，一般都是放在某个元素节点下面，获取节点的方式：
>
> 1. innerHtml属性，会将当前节点下的所有子节点转换成文本，因此修改时需要注意子元素节点的结构，否则会直接删除所有子节点。
>
> 2. innerText属性：读取时，只会读取文本(提取子节点中的文本)，忽略元素节点，写入也是会忽略节点。
>
> 3. textContent属性：忽略css效果，读取真实的文本，即会读取空格、换行以及隐藏元素中的文本。
>
> 4. childNodes属性，返回的是一个Node集合，需要自己去找到对应文本节点，然后通过nodeValue来修改文本。
>
> innerHtml与innerText说明：
>
> 1. 读取时：
>    1. innerHtml，返回字符串，会以子元素标签+子元素标签的内容文本的形式返回。
>    2. innerText，返回字符串，只提取文本节点的文本+子元素标签中的内容文本。
> 2. 写入时：
>    1. innerHtml，字符串中携带标签时，相当于给当前元素添重新设置子节点。
>    2. innerText，字符串中携带标签时，直接忽略，只提取文本。
>
> innerText与textContent说明：
>
> 1. textContent，显示文本包含了空格、换行以及隐藏元素中的文本。
> 2. innerText，显示的文本会删除空格、换行和隐藏元素直接忽略。
>
> 浏览器兼容性：
>
> innerText和innerHtml所有浏览器都支持，但是textContent属性，IE8及以下版本不支持，其他浏览器支持，因此需要做兼容性处理。

```html
<!DOCTYPE html>
<html>
    <body>
        <!-- 注释-->
        <div class="div1">
            <p class="ppClass">aaaa</p>
            <p id = "pId">bbbb</p>
        </div>

        <div class="div2">
            <p class="ppClass">aaaa</p>
            <p id = "pId">bbbb</p>
        </div>
        <script>
            var div1 = document.querySelector('.div1');
            var div2 = document.querySelector('.div2');
            console.log(div1.innerHTML);
            console.log(div2.innerText);
            console.log(div2.textContent);

            div1.innerHTML = '<span style="background-Color:red">hello world!</span>';
            div2.innerText = '     Hello World;'
          
          	// textContent属性的浏览器兼容性处理。
          	var text;
          	if(div2.textContent){
              text = div2.textContent;
            }else{
              text = div2.innerText;
            }
        </script>
    </body>
</html>
```



### 元素节点的操作



#### 元素节点的查询操作 

> 元素节点查询方法：
>
> 1. document.getElementById('idName');  根据标签的id属性来获取标签元素，返回的是Node类型的子类型对象。
> 2. document.getElementsByClassName('className'); 根据标签的class属性获取标签元素，返回HtmlCollection类型对象，是一个伪数组。
> 3. document.getElementsByTagName('p'); 根据标签的名称获取标签元素，返回HtmlCollection类型对象，是一个伪数组。
> 4. document.querySelector('css选择器'); 根据css选择器名称获取标签元素，返回找到的第一个标签元素。是一个Node类型的子类型对象。
> 5. document.querySelectorAll('css选择器'); 根据css选择器名称获取标签元素，返回NodeList类型对象，是一个伪数组。
>
> 优缺点：
>
> 1. 缺点是后两种IE8及以下不兼容下面的两个方法，其他浏览器兼容。
> 2. 优点灵活性比前三种类型强太多，利用css选择器精准定位的特点，大大提高了获取目标元素的准确性。
>
> 下面的例子就说明了后两种方式的灵活性：获取第一个div下的className为ppClass的p标签。

```html
<!DOCTYPE html>
<html>
    <body>
        <!-- 注释-->
        <div class="divs">
            <p class="ppClass">aaaa</p>
            <p id = "pId">bbbb</p>
        </div>
        <div>
            <p class="ppClass">ccc</p>
            <p>ddd</p>
        </div>
        <script>
            // 返回一个Node类型的子类型对象
            var pNodeOfId =  document.getElementById('pId');
            // 返回一个HtmlCollection类型集合对象，是一个伪数组
            var pNodeListOfTagName = document.getElementsByTagName("p");
            console.log(pNodeListOfTagName);
            // 返回一个HtmlCollection类型集合对象，是一个伪数组
            var pNodeListOfClassName = document.getElementsByClassName('ppClass');
            console.log(pNodeListOfClassName)

            // 返回一个Node类型的子类型对象
            var pNodeOfSelector = document.querySelector('.divs .ppClass');
            console.log(pNodeOfSelector);
            // // 返回一个NodeList类型集合对象，是一个伪数组
            var pNodeOfSelectorAll = document.querySelectorAll('.divs .ppClass');
            console.log(pNodeOfSelectorAll);
          
					 // 返回一个NodeList集合对象，保存所有div的第一个p标签。
            var pNodeOfDivFirstChild = document.querySelectorAll('.div p:nth-of-type(0)');
            console.log(pNodeOfSelectorAll);
        </script>
    </body>
</html>

运行结果：
[p.ppClass, p#PId, p.ppClass, p] // HtmlCollection
[p.ppClass, p.ppClass]  // HtmlCollection
<p class="ppClass">aaaa</p>
[p.ppClass]  // NodeList
```

#### 元素节点的增、删、改操作

> DOM提供了三种添加节点的操作：
>
> 1. document.write() 
> 2. objNode.innerHtml()
> 3. document.createElement()和document.appendChild()
>
> 三种添加方式优缺点：
>
> 1. 前两种的缺点在于会将之前的内容覆盖，例如document.write()方法，会将整个文档树替换成参数中的html文本；objNode.innerHtml()方法，会将当前节点的所有子节点替换成参数中的html文本。
> 2. 第三种是常用方式，创建一个节点并将其添加到父元素节点中，不会出现覆盖影响其他节点。
>
> 元素节点其他操作：
>
> 1. 父元素节点.insertBefore(新节点，参照节点); // 在参照子元素节点前添加新节点，返回新添加的元素节点
> 2. 父元素节点.replaceChild(新节点，旧节点)。  // 返回旧节点
> 3. 父元素节点.removeChild(需要被删除的节点)  // 返回删除的节点
> 4. 父元素节点.appendChild(需要被追加的节点)。// 返回追加的节点
> 5. 当前元素.remove() // IE不支持该方法，不需要使用父节点删除元素。

```html
<!DOCTYPE html>
<html>
    <body>
        <!-- 注释-->
        <div class="divs">
            <p class="ppClass">aaaa</p>
            <p id = "pId">bbbb</p>
        </div>
        <script>
            
            var divNode = document.querySelector('.divs');
            var pClassNode = document.querySelector('.divs .ppClass');
            var pClassIdNode = document.querySelector('#pId');

            // 追加元素节点
            var appendNode = document.createElement('p');
            appendNode.innerText = 'cccc';
            appendNode.style.backgroundColor = "red";
            var temp = divNode.appendChild(appendNode);
            console.log(temp);   // <p style="background-color: red;">cccc</p>


            // 插入元素节点
            var insertNode = document.createElement('p');
            insertNode.innerText = '1111';
            insertNode.style.backgroundColor = "green";
            temp = divNode.insertBefore(insertNode, pClassNode);
            console.log(temp);  // <p style="background-color: green;">1111</p>

            // 删除元素节点
            temp = divNode.removeChild(pClassIdNode);
            console.log(temp);  // <p id = "pId">bbbb</p>

            // 替换元素节点
            var replaceNode = document.createElement('p');
            replaceNode.innerText = '2222';
            replaceNode.style.backgroundColor = "blue";
            temp = divNode.replaceChild(replaceNode, pClassNode);
            console.log(temp); // <p class="ppClass">aaaa</p>

        </script>
    </body>
</html>
```



### 属性节点操作

> 属性是用来修改元素节点的，因此需要通过标签元素来获取属性，操作属性方式：
>
> 1. getAttribute()和setAttribute()，读取和设置属性值
> 2. getAttributeNode()和setAttributeNode()，获取和设置属性节点
> 3. 通过DOM提供的属性，例如id，className，style属性。
>
> 注意：对于自定义的属性，只能通过前两种方式来操作，style属性只能操作html协议中定义的属性值。

```html
<!DOCTYPE html>
<html>
    <body>
        <!-- 注释-->
        <div class="div1" customAttribute="hello">

        </div>

        <script>
            var div1 = document.querySelector('.div1');
            div1.style.name = 'SD'
            console.log(div1.style.name, div1.className);
          
            // 获取自定义属性值
            console.log(div1.getAttribute('customAttribute'));
            // 设置自定义属性值
            div1.setAttribute('customAttributedd', 'world');
            console.log(div1.getAttribute('customAttribute'));
            
            // 创建自定义属性节点
            var attributeNode = document.createAttribute("create");
            attributeNode.nodeValue = "Bus";
            div1.setAttributeNode(attributeNode);
            console.log(div1.getAttribute('create'));
        </script>
    </body>
</html>
```



## JS修改标签样式

> JS有三种修改标签样式的方式：
>
> 1. 通过style属性，修改标签样式。
> 2. 通过替换class属性值，修改标签的样式。
> 3. 通过修改css中的属性，修改标签的样式。
