# 类型转换

类型转换`修改的是变量的类型`，不是实例对象中的属性值，也不是实例对象类型。



# is 与as的使用

> is：用来进行类型判断，判断当前类是否是某个类型
>
> as：用来进行类型转换，将某个类转换成其真实类型(一般出现在父类变量指向子类实力对象的情况)

# Any与AnyObject的使用



> Any：代表任何数据类型，包括可选类型
>
> AnyObject：代表任何类类型

```swift
// Any类型数组
var arrs = [Any]();
// 保存整型
arrs.append(12)
// 保存字符串
arrs.append("abcds")
// 保存布尔型
arrs.append(true)
// 保存类类型
arrs.append(A())
// 保存闭包
arrs.append({ (name: String) -> String in "Hello, \(name)" })

// 保存可选类型
var a:[Int]? = [1,2,3,4,5]
arrs.append(a as Any)

for item in arrs{
    switch item{
        case is Int:
        print("item = \(item as? Int)")
    case is String:
        print("item = \(item as? String)")
    case is Bool:
        print("item = \(item as? Bool)")
    case is A:
        print("item = \(item as? A)")
    case is (String) -> String:
        print("item = \(item as? (String)->String)")
    case  is [Int]:
        print("item = \(item as? [Int])")
        
    default:
        print("不确定类型")
    }
}

打印结果：
item = Optional(12)
item = Optional("abcds")
item = Optional(true)
item = Optional(Test.A)
item = Optional((Function))
item = Optional([1, 2, 3, 4, 5])

代码分析：
保存可选类型时，使用as将其转换成Any类型，为了不让编译器报警告
```

