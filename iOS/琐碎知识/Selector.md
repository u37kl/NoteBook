# Selector

方法选择器，通俗的讲就是方法简称，iOS中类对象中使用`字典形式`保存实例对象需要调用的方法，`key：方法简称Selector，value：方法实现implement`，实例对象调用方法时，通过selector去类对象的方法列表中查询，获取对应的实现。

> 方法选择器中就是保存了一个字符串

## OC中如何使用

> 方法选择器类型为`SEL`，使用`@selector()`转换成方法选择器，其实方法简称就是方法名，将方法声明中的返回值和参数列表删除但`保留冒号`，将剩余字符串去掉空格后拼接在一起。

```objective-c
// 方法声明
-(NSString *) testWithFirst:(NSString*)firstStr sencond:(NSString*)secondStr

// 该方法的方法选择器
SEL = @selector(testWithFirst:second:)
```



## Swift中如何使用

>  swift中方法选择器类型为`Selector`，使用`#selector()`转换成方法选择器
>
> 两种方式创建方法选择器：
>
> 1. let a = #selector()
> 2. let a = Selector()

```swift
// 方法声明
class Person{
  // 如果需要OC调用该方法时，必须使用@objc修饰
  @objc func test(first a:String, second b:String)
}


// 写法一
let a = #selector(test(_:, _:))
// 写法二
let a = #selector(Person.test)
// 写法三，方法简称 = 方法名+参数(参数使用冒号表示)
let a = Selector("test::")
```



> 使用注意：
>
> 1. swift语言是静态语言，如果想让swift中定义的方法可以被OC调用，则必须使用`@objc`说明，例如按钮、通知触发方法调用都是涉及到动态调用，需要在使用@objc修饰该方法。`获取方法的Selector的情况都需要使用@objc来修饰。`

```swift
// 由于Student类的student方法支持运行时调用(按钮点击后，才查找方法实现)，因此需要方法需要@objc
class Student{
  @objc
  func student(){ }
}

let btn = Button()
btn?.addTarget(self, action: #selector(student), for: UIControl.Event.touchUpInside)
```



## 思考题(selector是否可以唯一标识一个方法)

```swift
import UIKit

class DemoViewController : UIViewController {
    
    override func viewDidLoad() {
        let view = DemoView()
        let btn = view.btn;
        self.view.addSubview(btn!)
    }
    
    @objc func btnTapped(_ sender: UIButton) {
        print("print: surprise!")
    }

}

class DemoView {
    var btn : UIButton!

    init() {
        btn = UIButton()
        btn.frame = CGRect(x: 110, y: 70, width: 100, height: 44)
        btn.backgroundColor = UIColor.blue
        btn.setTitle("Press me", for: .normal)
        btn.setTitle("I'm Pressed", for: .highlighted)
        btn.addTarget(self, action: #selector(DemoView.btnTapped(_:)), for: .touchUpInside)
    }
    
    @objc func btnTapped(_ sender: UIButton) {
        print("print: demo button is tapped")
    }
    
}
```



> 代码情况分析：
>
> 1. 按钮点击方法是使用`DemoView类的btnTapped`生成的方法选择器，点击按钮时应该调用DemoView的方法。
> 2. DemoView没有添加到控制器View中，而是将DemoView的btn属性添加到根View中。
>
> 答案：
>
> 调用DemoViewController的btnTapped方法。

>原理分析：
>
>1. Selector上面说了只是一个方法简称，是通过方法声明提取出来的，不同类允许创建相同方法声明的方法，selector不能唯一标识一个方法，就像人名一样，你叫张三我也叫张三。
>2. self具体原理不清楚，应该在按钮点击时才会去确定self指代的实例对象。

> 避免上面这种情况，导致self错乱。

## 封建迷信

> 错误知识：
>
> 1. #selector选择的类必须继承NSObject，也就是要写成class DemoView : NSObject
>    1. 答案：错误，Student和Person都没有继承NSObject类。
> 2. #selector中添加些方法格式必须像btnTapped(_:)这样。
>    1. 答案：错误，创建Selector可以使用写法二和三。