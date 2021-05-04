# 在没有泛型的情况

where子句用来限制协议的使用范围，

1. `限制在哪些数据类型中`
2. `限制在给哪些数据类型提供协议的默认实现`



## 限制实现协议的类

在协议定义时，设置该协议只能被哪些数据结构实现

```swift
protocol PersonProtocol where Self: Person{
  func person
}

class Person: PersonProtocol{
  func person(){ }
}
```



## 限制协议扩展给哪些数据类型提供默认实现

```swift
protocol TestProtocol{
  func test()
}

class Person{
  
}

class TestClass{
  
}

extension TestProtocol where Self: Person{
  func test(){
    print("TestProtocol --- test")
  }
}

extension Person: TestProtocol{ }
var p = Person()
p.test()

extension TestClass: TestProtocol{
  func test(){
    print("TestClass --- test")
  }
}
var t = TestClass()
t.test()

```



# 在引入泛型后的情况

针对扩展泛型类型的功能时，由于添加的功能会适用于所有符合类型形式参数的实际类型，因此需要使用where子句针对通过`扩展`或者`协议`添加的功能进行约束，规定添加的功能适合哪些泛型的实际数据类型。



## 给满足条件的泛型类型添加内容

设置Person类的泛型参数为Point时，才能调用扩展中的printPoint()发法

```swift
struct Point{
    var x:Int
    var y:Int
}

class Person<T>{
    var param:T
    init(param: T) {
        self.param = param
    }
}

extension Person where T == Point{
    func printPoint(){
        print("x= \(param.x), y= \(param.y)")
    }
}


var point = Point(x: 50, y: 50)
var p = Person<Point>(param: point)
p.printPoint()

// p1无法调用printPoint
var p1 = Person<Int>(param: 12)
```



设置Person类的泛型参数时Test或者Test的子类时，才能调用扩展中的printTest()方法

```swift
class Test{
    var a:String
    var b:String
    init(a:String, b:String) {
        self.a = a
        self.b = b
    }
}

class Person<T>{
    var param:T
    init(param: T) {
        self.param = param
    }
}


extension Person where T: Test{
    func printTest(){
        print("x= \(param.a), y= \(param.b)")
    }
}

var t = Test(a: "aaa", b: "bbb")
var p = Person<Test>(param: t)
p.printTest()

// p1无法调用printTest
var p1 = Person<Int>(param: 12)
```



## 给满足条件的泛型类型添加协议



```swift
struct Point{
    var x:Int
    var y:Int
}

class Person<T>{
    var param:T
    init(param: T) {
        self.param = param
    }
}

protocol PersonProtocol{
   func printPoint() 
}

extension Person: PersonProtocol where T == Point{
    func printPoint(){
        print("x= \(param.x), y= \(param.y)")
    }
}

var point = Point(x: 50, y: 50)
var p = Person<Point>(param: point)
p.printPoint()

// p1无法调用printPoint
var p1 = Person<Int>(param: 12)

```



```swift
class Test{
    var a:String
    var b:String
    init(a:String, b:String) {
        self.a = a
        self.b = b
    }
}

class Person<T>{
    var param:T
    init(param: T) {
        self.param = param
    }
}

protocol PersonProtocol{
   func printPerson() 
}

extension Person where T: Test{
    func printPerson(){
        print("x= \(param.a), y= \(param.b)")
    }
}

var t = Test(a: "aaa", b: "bbb")
var p = Person<Test>(param: t)
p.printTest()

// p1无法调用printTest
var p1 = Person<Int>(param: 12)
```





