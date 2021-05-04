# 何为泛型

为了提高类或者方法处理问题的广度和代码的复用性，提出了泛型这个新的概念。对数据类型进行抽象，变成一个占位符，根据实际调用时的数据类型来替代这个占位符，达到多个问题统一处理方法。

举例说明：

> 比较方法，如果不用泛型，那么对于每一个数据类型，都必须创建一个比较方法，使用泛型后，对比较的数据类型抽象成一个占位符，在实际调用时，将其替换成实际的数据类型，进行比较，这样大大提高代码复用性。



## 类型形式参数

泛型实现其实就是给数据类型定义`类型形式参数`，用类型形式参数代替实际的数据类型，充当一个占位符作用，swift编译器不会检查类型形式参数。

类型形式参数命名规则：可以使用`单个大写英文字母`，或者使用`单个首字母大写英文单词`。



## 泛型应用

### 泛型方法

泛型参数的作用范围在方法内部，在实际调用方法时根据传入实际参数类型来替换类型形式参数

> 泛型定义位置：定义在方法名后面使用<>包裹，多个类型形式参数使用逗号隔开

```swift
func swapTwoValue<T>(a: inout T, b: inout T){
  var temp = a
  a = b
  b = temp
}

func test<T, Element>(a:T, b:Element){
  print("\(a) --- \(b)")
}
```



### 泛型下标

> 下面代码表示查询学生名单中，待在这个班级中的学生

```swift
class Student{
    var name:String
    init(_ name:String) {
        self.name = name
    }
}

class Classes{
    var names:[Student] = [Student]()
    subscript<T:Student>(arr:[T])->[T]{
        var arrs = [T]()
        for stu in names{
            for item in arr{
                if stu.name == item.name{
                    arrs.append(item)
                }
            }
        }
        return arrs
    }
}

var stu1 = Student("Aaa")
var stu2 = Student("Bbb")
var stu3 = Student("Ccc")
var stu4 = Student("Ddd")

var classes = Classes()
classes.names = [stu1,stu2,stu3]
var stuArrs = [stu1,stu2,stu4]
stuArrs = classes[stuArrs]
print(stuArrs)
```



### 泛型类型



> 1. 泛型类型使用范围：自定义类、结构体、枚可以定义泛型类型
> 2. 泛型类型中的类型形式参数作用域：可以在当前类型定义内部和扩展中使用
> 3. 调用泛型类型时赋值：调用泛型类型时，需要设置类型形式参数的值，下面代码中`var stack = Stack<String>()`，设置stack结构体的类型形式参数为String类型

```swift
struct Stack<Element>{
    var items = [Element]()
    mutating func pushElement(item:Element) -> Bool {
        self.items.append(item)
        return true
    }
    
    mutating func popElement() -> Element? {
        guard items.isEmpty else {
            return nil
        }
        return self.items.removeLast()
    }
}

var stack = Stack<String>()
stack.pushElement(item: "1111")
stack.pushElement(item: "22222")
stack.popElement()
```



### 泛型协议 -- 关联类型

关联类型：在协议中定义的类型形式参数，使用`associatedtype`关键字

实现协议时设置关联类型实际类型方式：

>1. 显式设置，通过`typealias`关键字，设置关联类型
>2. 隐式设置，实现协议中的方法，使用具体的实际类型参数替换占位符，swift编译器会进行类型推断出关联类型的具体类型。



```swift
protocol TestProtocol{
    associatedtype Element // 关联类型
    func firstFun(t:Element) -> Element
}

class TestClass: TestProtocol{
    typealias Element = String
    func firstFun(t: String) -> String {
        return t + "--asdb"
    }
}
```

### 扩展中使用泛型

如果原类中定义了类型参数，那么在该类型的扩展中可以直接使用，不需要再定义新的类型形式参数

```swift
class Student<T>{
    var name:T
    init(name:T) {
        self.name = name
    }
}

extension Student{
    func test(t:T) {
        self.name = t
        print("name = \(self.name)")
    }
}

struct Information {
    var className:String
    var grade:Int
}


var stu = Student<Information>(name: Information(className: "高一年级一班", grade: 90))
stu.test(t: Information(className: "高一年级四班", grade: 85))
```





## 泛型约束

不存在可以处理任何数据的方法或者类型，因此需要对方法或者类型中定义的类型形式参数进行约束。

### 通过继承方式约束



#### 约束类型形式参数

通过继承方式，限制Test类中T、B两个类型形式参数的约束，限制firstFunc方法中的类型形式参数

```swift
class Person{
    
}

class Classes{
    
}

protocol PersonProtocol{
    
}


class Test<T:Person, B:PersonProtocol>{
    func firstFunc<Element:Classes>(cls: Element){
        
    }
}

```



#### 约束关联类型

使用`:`在声明关联类型时设置约束。

```swift
class Classes{
    var name:String = ""
}

protocol PPSP{
    associatedtype T:Classes
    func perproTest(t:T)
}

class Test<S:Classes>: PPSP{
    
    typealias T = S
    func perproTest(t: T){
        print(t.name)
    }
}

var tes = Test()
var cls = Classes()
cls.name = "aasa"
tes.perproTest(t: cls)

代码分析：
由于限制关联类型必须是CLasses或者其子类，因此Test遵循PPSP协议时，定义类型形式参数要实现S==PPSP.T，必须将类型形式参数S设置为CLasses或者其子类。
```



### where子句约束

在swift中的where子句文章中说明

