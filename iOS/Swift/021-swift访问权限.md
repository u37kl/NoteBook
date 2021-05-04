# 访问权限

访问权限的提出就是为了限制功能调用和数据的访问，它和其他的设置访问权限不一样(数据库访问权限)，它是通过给代码中的每个元素设置一个可以访问的范围，你在这个圈子中则可以访问。

swift提供了5种圈子大小，它们都是包含与被包含关系。



# 模块与源文件

swift将程序内容分为模块与源文件：

> `源文件`：它就是保存源代码的单个文件。
>
> `模块`：这里的模块指的是一个框架(静态库或者动态库)或者一个应用程序，我们使用的应用程序很多都是多个应用程序组合起来的。例如程序中使用的地图、支付、通讯等等。

# 5种访问级别

访问级别：用来限定类型的访问范围，只有在其范围范围内，才能范围该类型或者数据。

> 有两个框架A和B，在A中定义了一个Person类
>
> open：最宽松的访问级别，允许其他框架使用或者继承本框架的内容，即B框架可以使用和继承Person类。
>
> public：允许其他框架`使用`但是`不能继承`本框架的内容，即B框架只能使用Person类。
>
> internal：只能在定义它的框架中使用和继承，即本框架的任何位置都能使用Person类。
>
> fileprivate：只能在定义它的源文件中使用和继承，即在定义它的源文件中的任何位置都能使用Person。
>
> private：只能在定义它的地方使用和继承。
>
> 
>
> `Private `<` fileprivate`<`internal`<`public`<`open`
>
> swift的默认访问级别为internal



```swift
// 源文件main.swift中

private class Person{
    fileprivate var name:String = "aaa"
    private var address:String = "bbb"
}

class Test{
    private var p:Person = Person()
  	fileprivate var name = "aaaa"
}

private var p = Person();
var t = Test()
print(t.name)
```

> private和fileprivate区别：
>
> 1. private修饰类时，该类和fileprivate一样，可以在定义它的源文件的任何地方使用。
> 2. private修饰类中成员时，这些成员只能在类中使用，而fileprivate修饰的类成员可以在本文件中使用。



# 5种访问级别的使用

默认访问级别设置：

1. 在不设置访问级别时，swift默认给其设置为internal。
2. 当数据类型的访问级别<public时，数据类型中的成员的访问权限==所属数据类型的访问级别，即当所属类型访问级别为fileprivate时，成员访问级别都是fileprivate。
3. 当数据类型访问级别>=public时，数据类型中的成员访问级别==internal，保证内部成员访问安全，需要放开哪个成员访问级别，自己进行设置。

## 类与成员之间的访问级别

>类本身的访问级别 `>=` 类中成员的访问级别。不设置成员访问级别时，遵循默认访问级别设置。

## 函数、方法、下标访问级别设置

函数、方法与下标的访问级别判定：

1. 函数、方法的访问级别`<=`参数列表和返回值类型的访问级别中最严的那个。
2. 如果是无参无返回值的函数、方法的默认访问级别 <= 所属类的访问级别，当类的访问级别>internal时，方法和函数的访问级别`==`internal
3. 函数和方法调用时，必须处于其访问级别规定的范围内，才可以调用。
4. 下标的访问级别 `<=`参数和返回值中最严的访问级别

```swift
fileprivate class Person{
     var name:String = "aaa"
}

class Test{
  fileprivate var arrp = [Person]()
  
  fileprivate subscript(_ i:Int)->Person{
    return arrp[i]
  }
    fileprivate func test(p:Person)->String{
        print(p.name)
        return p.name
    }
}

代码分析：
test方法的参数和返回值中最严的那个访问级别是fileprivate，因此test方法的访问级别为fileprivate。
```



## 常量、变量、属性

1. 常量、变量和属性的访问级别`<=`它所指向的数据类型的访问级别。
2. 

可以给属性的setter方法单独设置访问权限，通过`private(set)`、`fileprivate(set)`、`internal(set)`这样的写法。

```swift
 public class Person{
    fileprivate private(set) var name:String = "aaa"
    
    public internal(set) var address:String{
        set{
            name = newValue
        }
        
        get{
            return name
        }
    }
}
```

## 初始化器访问级别设置

> 普通初始化器的访问级别的设置与方法、函数访问级别的设置一样。
>
> 必要初始化器：必要初始化器的访问级别 `== `所属类的访问级别。
>
> 默认无参初始化器：它的访问级别`==`所属类的访问级别。

## 枚举与元组

1. 元组的访问级别 `== `元组中最严的哪个元素的访问级别
2. 枚举的访问级别 `<=` 原始值和关联值的访问级别。

## 子类

子类的访问级别 `<=` 父类的访问级别

子类中重写的方法、属性可以`大于`父类方法的访问级别，但是必须`小于`子类本身的访问级别。

子类调用父类方法时，子类方法必须在父类方法的访问范围内，子类方法才能调用父类方法。

## 协议访问级别设置

协议访问级别规则：

1. 协议中声明的内容的访问级别必须 `==`协议本身的访问级别，即协议如果时public，则成员也是public，不再是internal了。
2. 协议继承时，子协议的访问级别`<=`父协议的访问级别



```swift
private protocol personProtocol{
    func person()
}

private protocol studentProtocol: personProtocol{
    func student()
}

fileprivate class Test{
    public func person(){
        print("person")
    }
    
    public func student(){
        print("student")
    }
}

```

>上面代码编译运行不报错，但违反了访问权限设置的规则：
>
>1. 类型成员的访问权限需要`<=`类型访问权限，像上面代码这样设置访问权限没有任何意义，其他源文件无法创建Test类的实例对象，类成员是public有什么意义。
>2. 遵循的协议实现协议中的方法时，修改了访问权限。使用协议就是像利用多态特性，类型实现方法的访问权限>协议中声明方法，用不了。



## 扩展访问级别设置

1. 扩展原类功能时，如果在原类成员的访问范围内，那么扩展就可以直接使用。
2. 给扩展设置访问级别，设置添加的功能的访问权限

```swift
// -- main.swift
var a = Test()
a.test()

// -- File.swift
class Test{
    var name:String = "aaaaa"
    func test(){
        self.testExtension()
    }
}

fileprivate extension Test{
    func testExtension(){
        print("testExtension --- \(name)")
    }
}

代码分析：
1. 扩展在Test中的name属性的访问范围内，因此可以在扩展中使用。
2. 由于扩展的访问权限时fileprivate，因此在main.swift中，无法使用Test类的扩展内容。
```

