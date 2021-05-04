

# React 路由基础

> React路由将React组件分成两种类型：`一般组件`和`路由组件`。
>
> 一般组件和路由组件的区别：
>
> 1. 生命周期不同，路由组件的显示与隐藏由当前URL地址决定，当路由组件的path与url的path不匹配时，路由组件将不会显示。一般组件的生命周期与项目的生命周期一致，随着项目的运行而生成，随着项目的停止而销毁。
> 2. props的参数不同，路由组件比一般组件多了`location`，`history`， `match`属性。

## 路由组件使用

### 搭建路由

> 路由的搭建分为两步：
>
> 1. 安装`react-router-dom`库，这个库用来实现React的前端路由
> 2. 使用`Route`标签，注册一个前端路由。
> 3. 使用`Link`标签，添加一个路由跳转按钮，让用户点击后进行路由跳转
>
> 搭建路由注意：
>
> 1. 想要使用前端路由，则使用`BrowserRouter`标签包裹React项目的根组件，App组件
> 2. 注册路由时，必须使用`Switch`组件包裹， `如果不加Switch就像Switch语句不加break一样`。

```jsx
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import {BrowserRouter} from 'react-router-dom' // 路由组件
import App from './App'
ReactDOM.render(
	<BrowserRouter>  // 如果想要使用路由，则必须将顶层组件使用BrowserRouter包裹。
		<App/>
	</BrowserRouter>,
	document.getElementById('root')
)
// App.js
import React, { Component } from 'react'
import {Route} from 'react-router-dom'
import Music from './components/Music'
import Movie from './components/Movie'

export default class App extends Component {
  render(){
    <Switch> // 注册路由时，必须放在Switch标签中，放在多次匹配
      <Route path="/Music" component={Music}/> // 注册登录页面的路由
      <Route path="/Movie" component={Movie}/>  // 组册主页面的路由
    </Switch>
  }
}
// Music.js
import React, { Component } from 'react'
import {Link,Route} from 'react-router-dom'
export default class Music extends Component {
  render(){
   	<div>
    		<Link to="/Movie">跳转到电影页面</Link>
    </div>
  }
}

// Movie.js
import React, { Component } from 'react'
import {Link,Route} from 'react-router-dom'
export default class Movie extends Component {
  render(){
   	<div>
    		<Link to="/Music">跳转到音乐页面</Link>
    </div>
  }
}
```



### 路由组件的模糊匹配与精准匹配

> 路由的匹配方式：
>
> ​		从项目的根组件开始寻找项目中的注册的路由组件(按照项目的组件树层级一层层寻找Route标签)，每找到一个就匹配Route的path属性，如果想要跳转的path完全包含Route的path属性值，则匹配成功，React则会渲染该路由组件。
>
> 下面示例介绍前端路由的模糊匹配
>
> 1. 当跳转的地址为/music/detail时，Music路由组件匹配成功，而不是MusicDetail路由组件，因为跳转的地址完全包含Music的path值，因此匹配成功。
> 2. 当跳转的地址为/movie/detail时，MovieDetail路由组件匹配成功。
>
> 路由组件模糊匹配问题的解决方式：
>
> 1. Route标签中添加`exact`属性，当Route添加了exact属性后，在进行匹配时，只有url的path完全与Route标签中的path值一致，才算匹配成功。
> 2. 当多个路由组件放在一起，将路径长的放在前面，路径短的放在后面，如果某个Route的path包含另一个Route的path，就像下面的Music和MusicDetail，要将MusicDetail放在Music前面，MovieDetail就放在Movie前面。

```jsx

    <Switch> // 注册路由时，必须放在Switch标签中，放在多次匹配
      <Route path="/music" component={Music}/> // 注册登录页面的路由
		  <Route path="/music/detail" component={MusicDetail}/> // 注册登录页面的路由
		  <Route path="/movie/detail" component={MovieDetail}/> // 注册登录页面的路由
      <Route path="/movie" component={Movie}/>  // 组册主页面的路由
    </Switch>
```



### 路由重定向

> 使用`Redirect`标签添加路由重定向，放在一组被Switch标签包裹的路由组件的后面，表示当没有找到匹配时跳转到默认的路由组件中。

```jsx
// 当路径为/user时，没有匹配到路由组件，默认跳转到路径为/about的路由组件中。

<Switch>
		<Route path="/about" component={About}/>
		<Route path="/Music" component={Music}/>
		<Redirect to="/about"/>
</Switch>
```



### 路由嵌套

> 路由嵌套，也就是二级路由，或者说n级路由.
>
> 当URL地址为`/movie/detail`时的匹配过程：
>
> 1. 从项目的根组件出发(也就是App)，进行匹配。
> 2. 在App组件中匹配到Music组件，则React将渲染Music组件。
> 3. Music组件中还注册了路由组件，则继续匹配，匹配到Detail组件，因此在Music组件中渲染Detail组件。
>
> 路由组件嵌套的嵌套：
>
> ​		拿一二级路由嵌套举例，二级路由的path必须包含一级路由的path，否则无法实现二级路由。

```jsx
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import {BrowserRouter} from 'react-router-dom' // 路由组件
import App from './App'
ReactDOM.render(
	<BrowserRouter>  // 如果想要使用路由，则必须将顶层组件使用BrowserRouter包裹。
		<App/>
	</BrowserRouter>,
	document.getElementById('root')
)

// App.js
import React, { Component } from 'react'
import {Route} from 'react-router-dom'
import Music from './components/Music'
import Movie from './components/Movie'

export default class App extends Component {
  render(){
    <Switch> // 注册路由时，必须放在Switch标签中，放在多次匹配
      <Route path="/music" component={Music}/> // 注册登录页面的路由
      <Route path="/movie" component={Movie}/>  // 组册主页面的路由
    </Switch>
  }
}

// Music.js
import React, { Component } from 'react'
import {Link,Route} from 'react-router-dom'
export default class Music extends Component {
  render(){
    <Switch> // 注册路由时，必须放在Switch标签中，放在多次匹配
      <Route path="/music/detail" component={Detail}/> // 注册登录页面的路由
			<Route path="/music/dpdate" component={Update}/> // 注册登录页面的路由
    </Switch>
  }
}

```



### 路由的三种传参方式

> 1. query，查询字符串，通过`this.props.match.params`接收数据
> 2. params，参数路由，通过`this.props.location.search`
> 3. state，对象，通过`this.props.location.state`接收数据

```jsx
// params
跳转 <Link to={`/Music/message/detail/${msgObj.id}/${msgObj.title}`}>跳转</Link>
注册 <Route path="/Music/message/detail/:id/:title" component={Detail}/>
组件中通过 this.props.match.params 接收父组件传递过来的参数

// query
跳转 <Link to={`/Music/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>跳转</Link>
注册 <Route path="/Music/message/detail" component={Detail}/>
组件中通过 this.props.location.search 接收父组件传递过来的参数，需要对数据进行urlencode解码

// state
跳转 <Link to={{pathname:'/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>跳转</Link>
注册 <Route path="/message/detail" component={Detail}/>
组件中通过 this.props.location.state 接收父组件传递过来的参数
```



### 编程式路由导航

> 路由切换方式：
>
> 1. 通过用户点击`Link`标签，切换路由。
> 2. 通过修改浏览器地址栏，切换路由。
> 3. 通过代码方式切换路由，这种方式被称为编程式路由导航
>
> React提高了路由切换函数：
>
> 1. replace(path,param)，使用当前方法中的path将浏览器历史记录的栈顶记录覆盖。
> 2. push(path, param)，将path添加到浏览器历史记录栈的栈顶。
> 3. go(number)，向后或者向前跳转
> 4. goForward，向前跳转
> 5. goBack，向后跳转

```jsx
		//replace跳转+携带params参数
		this.props.history.replace(`/Music/message/detail/${id}/${title}`)

		//replace跳转+携带search参数
		this.props.history.replace(`/Music/message/detail?id=${id}&title=${title}`)

		//replace跳转+携带state参数
		this.props.history.replace(`/Music/message/detail`,{id,title})


		//push跳转+携带params参数
		this.props.history.push(`/Music/message/detail/${id}/${title}`)

		//push跳转+携带search参数
		this.props.history.push(`/Music/message/detail?id=${id}&title=${title}`)

		//push跳转+携带state参数
		this.props.history.push(`/Music/message/detail`,{id,title})
```



### withRouter

> 由于一般组件的props没有`location`，`history`， `match`属性，但是需求中由需要使用这些使用时，就需要借助withRouter方法将一般组件包装成路由组件，实现在一般组件中也可以使用路由组件的一些属性和方法。(例如有些需求中需要根据地址栏的路径来显示该组件的标题)

```jsx
import React, { Component } from 'react'
import {withRouter} from 'react-router-dom'
class Music extends Component {
  render(){
   	<div>
    		<Link to="/Movie">跳转到电影页面</Link>
    </div>
  }
}

export default withRouter(Music) // 使用withRouter这个高阶组件将一般组件包装成路由组件。
```



### 路由的懒加载

> 一般用户在方法这个项目时，会将该项目的所有静态资源下载下来，即使某些组件没有使用，这样就会造成不必要的流量和时间上的浪费，因此React提供了路由的懒加载，或者说是路由的按需加载。
>
>  
>
> 路由组件懒加载编写注意：
>
> 1. 注册的懒加载的路由组件必须包裹在`<Suspence>`标签中，也就是说在所有的`Switch`标签外层在包裹一层`<Suspence>`标签。
> 2. `<Suspence>`标签中的fallback中可以编写一个组件，用来作为路由组件懒加载时的loading显示，需要注意这个组件不能使用懒加载，否则报错。

```jsx
// App.js
import React, { Component, lazy } from 'react'
import {Route} from 'react-router-dom'

const Music = lazy( () => import('./components/Music') );
const Movie = lazy( () => import('./components/Movie') );

export default class App extends Component {
  render(){
    <Link to="/music">打开音乐页面</Link>
    <Link to="/movie">打开电影页面</Link>
    
    <Suspense fallback={<h1> 组件正在加载中....</h1>}>
    	<Switch> // 注册路由时，必须放在Switch标签中，放在多次匹配
      		<Route path="/music" component={Music}/> // 注册登录页面的路由
		      <Route path="/movie" component={Movie}/>  // 组册主页面的路由
  	  </Switch>
    </Suspense>
  }
}
```

