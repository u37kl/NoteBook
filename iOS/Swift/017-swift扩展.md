# 扩展的功能

扩展格式

```swift
extension 数据结构名称: 协议名称{
  内容
}
```



> 扩展中添加的内容：
>
> 1. 计算型属性(实例属性和类型属性)
> 2. 便携构造函数
> 3. 添加方法
> 4. 添加下标
> 5. 添加内嵌类型



## 添加计算型属性

swift扩展属性添加规则：

> 1. 只能给原类添加计算型属性
> 2. 不能重写父类的属性，不管是存储属性还是计算型属性

```swift
class Person{
    var name = ""
}

class Student: Person{
    
}

extension Student{
    var desc:String{
        return super.name
    }
}

var s = Student()
print(s.desc)
```





## 提供初始化器



### 扩展值类型初始化器

值类型默认初始化器有两个，`无参初始化器`和`成员初始化器`，因此在扩展初始化器时，不能定义原类已存在的初始化器。



扩展值类型初始化器的情况：

>1. 原值类型定义时属性设置默认值，并且没有提供任何初始化器，系统提供两个初始化器，无参和成员初始化器，因此扩展中不能创建无参和成员初始化器。
>
>2. 原值类型定义时属性`没有`设置默认值，并且没有提供任何初始化器，系统只提供成员初始化器，因此扩展中不能创建成员初始化器。
>
>下面的代码说明上面这两句话的意思。

```swift
struct ZPPoint{
    var x:Int = 0
    var y:Int = 0
}

extension ZPPoint{
    
    
    init(a:Int, b:Int) {
        self.x = a
        self.y = b
    }
}

struct ZPSize{
    var width:Int
    var height:Int
    
    init(width:Int, height:Int) {
        self.width = width
        self.height = height
    }
}

extension ZPSize{
    
    init() {
        self.width = 0
        self.height = 0;
    }
    
    init(a:Int, b:Int) {
        self.width = a
        self.height = b
    }
}

代码分析：
ZPPoint结构体中符合第一种情况，系统创建了init()和init(x:Int, y:Int)两个初始化器，因此需要创建不同的初始化器，根据方法识别规则，修改参数名称、调整参数顺序等方法。

ZPSize结构体符合第二种情况，系统创建了init(width:Int, height:Int)初始化器，因此可以创建无参初始化器和其他参数名称的初始化器。
```





### 扩展引用类型初始化器

扩展只能添加`便携初始化器`，并且不能在扩展中重写父类初始化器。

```swift
class Person{
    var name = ""
    var ss:String{
        return self.name
    }
    init(name:String) {
        self.name = name
    }
}

class Student: Person{
    
}

extension Student{
   convenience init(s:String) {
        self.init(name: s)
    }
}
```



## 添加方法



使用扩展添加循环

```swift
extension Int{
    func repeats(_ fun:() -> Void){
        for _ in 0 ..< self{
            fun()
        }
    }
}

3.repeats {
    print("hello world")
}

打印结果：
hello world
hello world
hello world
```

使用扩展添加mutating方法

```swift
extension Int{
    mutating func doubleFunc(){
        self = self * 2
    }
}

var t = 3
t.doubleFunc()
print(t)

打印结果：
6
```



## 添加下标

使用下标获取数字中某位的值，在定义下标的参数列表时，可以直接省略外部参数名。

```swift
extension Int{
    subscript(_ index:Int) -> Int{
        var num = self
        for _ in 0..<index{
            num = num / 10
        }
        return num % 10
    }
}

print(123456789[0])  // 个位
print(123456789[1])  // 十位
print(123456789[2])  // 百位
print(123456789[3])  // 千位
print(123456789[4])  // 万位

extension Int{
    subscript(s index:Int) -> Int{
        var num = self
        for _ in 0..<index{
            num = num / 10
        }
        return num % 10
    }
}
print(12345[s:1])
p

打印结果：
9
8
7
6
5
4
```



## 添加内嵌类型

给Int类型添加内嵌枚举，分为正数、负数和0

```swift
extension Int {
    enum Kind {
        case negative, zero, positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .zero
        case let x where x > 0:
            return .positive
        default:
            return .negative
        }
    }
}

func printIntegerKinds(_ numbers: [Int]) {
    for number in numbers {
        switch number.kind {
        case .negative:
            print("- ", terminator: "")
        case .zero:
            print("0 ", terminator: "")
        case .positive:
            print("+ ", terminator: "")
        }
    }
    print("")
}
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
```