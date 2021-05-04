# 协议定义和遵循协议的格式

协议定义格式：

```swift
protocol 协议名: 父协议名, 父协议名2{
  协议内容
}

```

遵循协议格式：

```swift
先写继承的父类，再写遵循的协议

class 类名: 父类名, 协议名{
  
}
```





# 协议的内容



## 属性

协议中的属性声明要求：

> 1. 协议中只能定义变量，不能定义常量
> 2. 实现时，协议中定义的get属性，实现时可以为`可读`或者`可读可写`的存储属性或者计算属性；协议中定义的set、get属性，实现时只能是`可读可写`的存储属性或者计算属性。
> 3. 协议中可以定义类型属性和实例属性。

```swift
protocol P1 {
    var a:String{get}
    var b:String{get}
    var c:String{set get}
}

struct T1: P1{
    
    var a:String{
        get{ return "111"}
    }
    var b:String{
        set{ }
        get{ return "111" }
    }
    
    var c:String{
        set{ }
        get{ return "111"}
    }
}

struct T2:P1 {
    var a: String = ""
    var b: String = ""
    var c: String = ""
}

代码分析：
协议中的c属性在实现时，不能定义为只读属性
```



## 方法



协议中的方法声明要求：

> 1. 方法声明可在数据结构中定义方法一样，只不过不能设置参数默认值
> 2. 协议中的mutating方法，类遵循协议时，实现方法不需要添加mutating，而结构体和枚举需要

```swift
protocol P1 {
    mutating func test1()
}

struct S1: P1{
    mutating func test1() {
        
    }
}

struct S2: P1{
    func test1() {
        
    }
}

class C1:P1 {
    func test1() {
        
    }
}

代码分析：
结构体S2没有将协议中的方法定义为mutating方法，那么不具有修改值类型属性的功能。
```



## 构造方法

协议中的初始化器：

> 1. 协议中只能定义指定初始化器，不能定义便携初始化器。
> 2. 遵循该协议的数据结构，实现时可以使用指定初始化器或者便携初始化器实现，但是初始化器前必须添加`required`关键字



可失败初始化器

> 虽然协议中声明了可失败初始化器，但是实现是可以使用可失败或者普通的初始化器
>
> 必须使用必要构造函数实现，即添加`required`关键字

```swift
protocol P1{
    init(a:Int)
    init(b:Int)
    init?(c:Int)
    init?(d:Int)
    init?(e:Int)
}

class C1:P1{
    // 定义为指定初始化器
    required init(a: Int) {  }
    // 定义为便携初始化器
    required convenience init(b: Int) {  }
  	// 定义为可失败初始化器
    required init?(c: Int) { }
  	// 定义为便携可失败初始化器
    required convenience init?(d: Int) { }
  	// 将可失败转化为不可失败
    required init(e: Int) { }
}
```



协议和父类声明了同名同类型构造方法时，子类重写父类构造方法使用`override`和`required`

```swift
protocol SomeProtocol {
     init()
}

class SomeSuperClass {
    init() {
        
    }
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" from SomeProtocol conformance; "override" from SomeSuperClass
    required override init() { }
}
```



# 协议作为类型

协议作为类型：

- 在函数、方法或者初始化器里作为形式参数类型或者返回类型；
- 作为常量、变量或者属性的类型；
- 作为数组、字典或者其他存储器的元素的类型。



# 协议的继承

协议的继承和类继承一样，放在协议名后面，多个协议使用逗号隔开。

```swift
protocol P1{ }
protocol P2{ }
protocol P3: P1, P2{ }

P3继承P1和P2协议
```



# 协议组合

现实中，存在一些数据类型遵循多个协议的情况，因此有些属性、方法参数、集合元素需要满足多个协议的数据结构，此时就用到了协议组合。

使用`&`连接多个协议

```swift
protocol P1{
    func test1() -> Void
}
protocol P2 {
    func test2() -> Void
}

class C: P1, P2{
    func test1(){
        print("C --- Test1")
    }
    
    func test2(){
        print("C --- Test2")
    }
}
class Test {
    
    var param: (P1 & P2)?
    
    func fun1(c: P1 & P2){
        print(c.test1())
        print(c.test2())
    }
}

var t = Test()
var c = C()
// 协议组合使用在方法参数重
t.fun1(c: c)
// 协议s组合使用在集合中
var arr = [P1 & P2]()
arr.append(c)
print(arr)

代码分析：
协议组合使用在属性定义、方法参数、集合定义中。
```



# 协议类型检查

由于协议可以作为类型，因此可以使用`is`和`as`来进行类型判断

```swift
protocol P1{
    func test1() -> Void
}
protocol P2 {
    func test2() -> Void
}

class C: P1, P2{
    func test1(){
        print("C --- Test1")
    }
    
    func test2(){
        print("C --- Test2")
    }
}

var c = C()
if c is P1{
    print(("c遵循P1"))
}
if c is (P1 & P2){
    print("c遵循P1和P2")
}

var p1 = c as P1
p1.test1()
var p2:(P1 & P2) = c as (P1 & P2)
p2.test2()
```



# 扩展与协议



## 使用扩展声明类型遵循协议

在扩展中遵循协议情况：

> 1. 原类中实现协议中所有内容，但没有遵循协议时，可以使用扩展遵循协议，例如C类
> 2. 原类中没有实现协议中的内容，使用扩展遵循协议并实现协议中的内容，例如C1类。



```swift
protocol P1{
    func test1() -> Void
}
protocol P2 {
    func test2() -> Void
}

class C{
    func test1(){
        print("C --- Test1")
    }
    
    func test2(){
        print("C --- Test2")
    }
}
extension C: P1, P2{ }

class C1 {
}

// 让C1遵循协议
extension C1:P11, P21{
    func test1() { }
    func test2() { }
}

var c = C()
c.test1()
c.test2()
```



## 协议扩展

使用协议扩展给协议中的方法和计算属性提供实现，通过协议方法给所有遵循或者将要遵循该协议的数据类型提供默认实现。

如果不满意协议扩展实现的内容，可以在数据类型中实现相应的方法或者属性，覆盖协议扩展中的内容。

```swift
protocol P1{
    var s:String{get}
    func test1() -> Void
}
protocol P2 {
    func test2() -> Void
}

extension P1{
    var s:String{
        get{
            return "123456"
        }
        
        set{ }
    }
    func test1(){
        print("C --- Test1")
    }
}

extension P2{
    func test2(){
        print("C --- Test2")
    }
}

class C1: P1, P2{

}
class C2: P1, P2{
    var s:String = "43212"
    func test1() {
        print("C2 --- test1")
    }
}

var c = C1()
c.test1()
c.test2()

var c1 = C2()
c1.test1()

代码分析：
对协议P1和P2添加协议扩展，实现其中的方法，遵循协议的C类型，不需要在自己实现这些方法，因为协议扩展中实现了。
C2中创建test1方法来覆盖协议扩展中定义的方法和属性
```





# 协议限制



## 类专属协议

限制遵循协议的数据类型为引用类型，使用AnyObject

```swift
protocol P1: AnyObject{
    func test1()
}

class C: P1{
    func test1() { }
}
```

协议继承下的类型专属协议

```swift
protocol P2{
    func test2()
}

protocol P3: AnyObject, P2 {
    func test3()
}

class C2: P3 {
    func test2(){ }
    func test3(){ }
}

struct S:P2 {
    func test2(){ }
}

代码分析：
某个协议遵循多个协议，并且要求该协议是类专属协议时，可以平常协议继承写法一样，但是第一个必须是AnyObject，像P3类
```



## 特定类的专属协议

用来限制只有某些类能遵循该协议，使用`where限制语句`

```swift

class C1{
    
}

protocol P where Self: C1{
    func test() -> Void
}

class C2 : C1, P{
    func test() {
        
    }
}

 编译报错，'P' requires that 'C3' inherit from 'C1'
class C3: P{
    func test() {
        
    }
}

代码分析：
P协议只能让C1和其子类遵循
```





## 限制协议扩展

协议扩展作用是给协议添加默认实现，让遵循该协议的数据类型不需要实现协议内容，而限制协议扩展用来限制`只给某些特定类和该类的子类添加默认实现`

使用`where限制语句`

```swift
protocol P1 {
    func test1()
}

class Test{
    
}

extension P1 where Self:Test{
    func test1(){
        print("P1 --- extension -- test1")
    }
}

class C1: P1{
    func test1(){
        print("C1 --- test1")
    }
    
}

class C2: Test, P1 {
    
}

var c1 = C1()
var c2 = C2()

代码分析：
扩展协议P1给Test和Test的子类提供了test1方法的实现，因此C1类需要自己实现test1方法，而C2不需要自己实现。
```



# 可选协议

可以在协议中定义可选协议，遵循协议的类可以不实现协议中的内容。可选协议必须使用`@objc`和`optional`关键字修饰。

被optional修饰的方法，为可选方法，方法类型为`(()->())?`

```swift
// 可选协议
@objc protocol TableDelegateProtocol{
    var a:Int{ get }
    @objc optional var b:Int{ get }
    @objc optional func getDataSource()
}

class TableDelegate: TableDelegateProtocol{
    var a:Int = 0
    func getDataSource() {
        print("获取数据源")
    }
}

class TableView{
    var delegate: TableDelegateProtocol?
    func test(){
        self.delegate?.getDataSource?()
    }
}
代码分析：

定义可选协议，test方法调用getDataSource方法成功前提：
1. delegate不为nil。
2. delegate对应的类实现了getDataSource方法。
```

