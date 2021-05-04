# 组件通信方式

> 组件之间的关系：
>
> 1. 父子组件。
> 2. 兄弟组件。
> 3. 由共同祖先没有共同父亲的组件。

## props方式实现组件通信

> 使用props实现父子组件之间的通信方式：
>
> 1. 父传子，通过给子组件添加属性。
> 2. 子传父，通过给子组件添加一个回调函数

### render props

> 组件的插槽或者给组件添加一个占位组件，该组件展示什么组件有props传递过来。

```jsx
class Parent extends Component{
  render(){
 	   <div>
      	<A render={(param)=><C data={param}/> }/> // 将组件C渲染到组件A中
     </div>
  }
}

class A extends Component{
  state = {name:'tom'}
	render(){
    return(
	      <div className='a'>
      		{this.props.render(this.state.name)} // 定义A组件的占位组件，这里展示什么有父组件传递过来。
      	</div>
    )
  }
}

class C extends Component{
  render(){
    return <h1> {this.props.data} </h1>
  }
}
```

> render-prop会破坏`React.PureComponent`，上面代码中每次Parent渲染时，都会创建一个新函数赋值给A组件的render属性，导致A组件的props永远在更新中，因此`shouldComponentUpdate`永远返回的true，也就是容易刷新A组件。

```jsx
// 改进版
class Parent extends Component{
  renderC(param){
    return <C data={param}/>
  }
  render(){
 	   <div>
      	<A render={ this.renderC }/> // 将组件C渲染到组件A中
     </div>
  }
}

class A extends Component{
  state = {name:'tom'}
	render(){
    return(
	      <div className='a'>
      		{this.props.render(this.state.name)} // 定义A组件的占位组件，这里展示什么有父组件传递过来。
      	</div>
    )
  }
}

class C extends Component{
  render(){
    return <h1> {this.props.data} </h1>
  }
}
```



## 消息订阅与发布

> Pub-sub

## 集中式管理

> redux

## 生产者消费者

> context方式，一般用在框架中

