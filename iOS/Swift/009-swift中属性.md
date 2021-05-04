# 属性种类

属性分为两种：`存储型属性`与`计算型属性`

> 使用范围：
>
> 存储型属性：只有`结构体`和`类`才能有存储属性
>
> 计算型属性：`结构体`、`枚举`和`类`可以有计算型属性

> 使用规则：
>
> 非可选类型存储属性：在类或者结构体`构造函数完成之前`，必须进行初始化，否则编译器报错。
>
> 可选类型存储属性：可选类型存储属性在调用构造方法时，如果没有设置特定值，默认初始化为nil。

## 存储属性

### 普通存储属性

```swift
class Test1: CustomStringConvertible{
    
    var a:Int = 12 // 定义初始化
    var b:Int  // 构造函数中初始化
    var c:String? // 可选类型，啥时候用啥时候初始化
    let d:Int
    
    init(_ b:Int, _ d:Int){
        self.b = b
        self.d = d
    }
    // 打印类实例信息
    var description: String{
        return "Test1: a= \(a) , b= \(b) , c= \(String(describing: c)) , d= \(d)"
    }
}

var t1 = Test1(11, 12)
print(t1)

打印结果：Test1: a= 12 , b= 11 , c= nil , d= 12

结果分析：
1. Test1中有3个存储属性，a与b属性都是在构造函数完成前进行了初始化，而c是可选类型，系统默认初始化为nil
2. 常量属性也必须在初始化完成前赋值，否则编译不通过
```



### 延时存储属性

实现非可选类型存储属性使用时再初始化，相当于懒加载。

```swift

class Test2{
    var a = 12
    init(_ param: String) {
        print("Test2类的init方法: \(param)")
    }
}
class Test3{
    var a:Int = 12
    var b = Test2("参数B")
    lazy var c = Test2("参数C")
}

var t2 = Test3()
var t3 = t2.c

// 结构体中定义延时存储属性
struct P{
    lazy var a = 12
}
var s = P()
print(s)
s.a = 12;
print(s)

打印结果：
Test2类的init方法: 参数B
Test2类的init方法: 参数C

P(a.storage: nil)
P(a.storage: Optional(12))

分析：
从上面可以看出，Test3构造方法调用完成后，参数C还没有进行初始化，只有当第一次访问参数c时，才触发参数c的初始化方法。
```

>总结使用延时型存储属性条件：
>
>1. 属性`必须是变量`，不能是常量，因为编译器要求常量属性必须在构造方法完成前完成初始化。
>2. 必须是`存储型属性`才能使用延时，使用lazy修饰。	
>3. 必须在`定义时`，设置初始化值，不能在构造函数或者其他地方设置初始化值。



## 计算型属性

​	计算属性`必须是变量`，不能是常量。计算型属性本身不存储数据，通过定义getter和setter方法来修改其他存储属性值

```swift
struct Point{
    var x:Int
    var y:Int
}

struct Rect {
    var origin:Point
    var size:Point
    var center:Point{
        get {
            let centerX = origin.x + (size.x) / 2
            let centerY = origin.y + (size.y) / 2
            return Point(x: centerX, y: centerY)
        }
        
        set {
            origin.x = newValue.x - (size.x) / 2
            origin.y = newValue.y - (size.y) / 2
        }
    }
    
    var edge: Point{
        set(newEdge){
            origin.x = newEdge.x - (size.x)
            origin.y = newEdge.y - (size.y)
        }
        
        get{
            let edgeX = origin.x + size.x
            let edgeY = origin.y + size.y
            return Point(x: edgeX, y: edgeY)
        }
    }
    
}

var rect = Rect(origin: Point(x: 50, y: 10), size: Point(x: 100, y: 100))
var center1 = rect.center
rect.center = Point(x: 50, y: 50)
var center2 = rect.center
print("\(center1) --- \(center2)")
```



# 属性监听

>使用条件：
>
>​	属性必须是`存储属性`；非重写的计算型属性不需要监听，通过getter和setter方法就可以完成监听。
>
>
>
>创建方式：
>
>​	实现对类或者结构体中存储属性的监听，通过给存储属性添加`willSet`和`didSet`方法，在修改前和修改后分别获取`旧值`和`新值`。
>
>
>
>获取监听数据：
>
>	1. willSet方法中通过`newValue`获取新值，参数名获取旧值
> 	2. didSet方法中通过`oldValue`获取旧值，参数名获取新值
>
>
>
>监听规则：
>1. 在实例对象初始化第一阶段时，属性监听不会被触发，换句话说在属性设初始值时不会触发监听。
>2. 其他所有方式修改属性值时，都会触发属性监听。
>
>



```swift
// 属性监听

class Test1{
    var a: Int = 12{
        willSet(newA){
            print("将要更新A参数：旧值：\(a) , 新值：\(newA)")
        }
        
        didSet{
            print("已经更新完A参数：旧值：\(oldValue)，新值：\(a)")
        }
    }
    
    // 省略willSet名称
    var b:Int = 10{
        willSet{
            print("将要更新A参数：旧值：\(a) , 新值：\(newValue)")
        }
        
        didSet{
            print("已经更新完A参数：旧值：\(oldValue)，新值：\(a)")
        }
    }
}

var t = Test1()
t.a = 12 // 值相同也触发属性监听
t.a = 13

func fun(_ a: inout Test1){
    a.a = 5
}

fun(&t)

打印结果：
	将要更新A参数：旧值：12 , 新值：12
	已经更新完A参数：旧值：12，新值：12
	将要更新A参数：旧值：12 , 新值：13
	已经更新完A参数：旧值：12，新值：13
	将要更新A参数：旧值：13 , 新值：5
	已经更新完A参数：旧值：13，新值：5

结果分析：
	值相同也会触发属性监听。
	结构体中存在属性监听，并且结构体作为函数的输入输出参数时，也会触发属性监听，因为输入输出原理就是函数返回时将输入输出参数在赋值回去。
```

>着重说明：输入输出参数
>
>输入输出形式参数的拷贝入拷贝出存储模型导致的：值一定会在函数结束后写回属性，从而触发属性监听
>
>



# 全局变量与局部变量

​		我们经常使用的全局变量和局部变量都是存储类型，但是也可以定义计算型全局变量或者局部变量。

​	全局变量都是延时存储类型，与延时存储属性一样，使用时才初始化。

```swift
// 定义一个计算型全局变量
var gloalVar: Int{
    get{
        return 12
    }
}

print(gloalVar)
```



# 类型属性(相当于类属性)



​	结构体和类不仅有实例属性，还有类型属性，类型属性的值被所有实例对象共享，并且只有一个，相当于实例成员变量和类成员变量



```swift
class Person{
    var t:Int = 12
    
    init() {
        print("---初始化Person类实例对象-----")
    }
}
class Gloal{
    static let a = "1111"  // 存储类型属性常量
    static var b = "2222"  // 存储类型属性变量 
    static var c: Int{     // 计算类型属性变量
        get{
            return 12
        }
    }
    class var d:Int{      // 计算类型属性变量
        return 14
    }
    static var p = Person() // 存储类型属性是具有延时性
}

print("\(Gloal.a) --- \(Gloal.b) --- \(Gloal.c)")
print(Gloal.p)


打印结果：
1111 --- 2222 --- 12
---初始化Person类实例对象-----
Test.Person // 打印的那个包下的类

```

>总结：
>
>1. 类型属性可以是变量，也可以是常量
>2. 类型属性可以是存储类型，也可以是计算类型
>3. 存储类型属性都具有延时性，即只有当使用时才会初始化。
>4. 创建类型属性使用`static`关键字，如果类型属性时计算型时，可以使用`class`关键字，允许子类重写父类类型属性中的getter和setter方法



# 属性使用注意

>结构体赋值给变量和常量是不一样的：
>
>赋值给变量：只要结构体中的属性不是let修饰，就可以修改。
>
>赋值给常量：结构体中的所有属性都不能修改。
>
>
>
>结构体和类中的属性区别：
>
>因为结构体有一个默认的构造方法，因此结构体中的属性在定义时不设置初始值也没有关系。
>
>类中的属性的初始化需要自己来解决，或是定义时添加初始值，或是定义构造方法并在其中初始化。





```swift
struct Point{
    var x:Int
    let y:Int
}

var p = Point(x:12, y:13)
p.x = 14

let p1 =  Point(x:12, y:13)
// p1.x = 14  由于结构体赋值给常量，因此所有属性都不能修改
```





# 属性包装

