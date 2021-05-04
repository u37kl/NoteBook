# 下标说明

下标的作用：实现快速访问数据结构中的内容，例如数组、字典都是使用了下标。

下标重载：类或结构体可以创建多个下标，并且针对同个属性建立多个下标，访问时根据中括号中的数据类型来判断调用哪个下标。

下标参数类型：下标定义可以看成没有名称的函数，支持除了默认参数和输入输出型参数外的所有参数类型，包括可变参数。



## 下标用法

```swift
struct Test{
    var num  = 1
    
    subscript(a:Int) -> Int{
        return num * a
    }
  
  	subscript(b:String) -> String{
      return "\(b)的内容为\(num)"
    }
}

var t = Test()
print(t[3])
print(t["AAAS"])

struct Arrays {
    var arr: [String]
    subscript(a:Int)->String{
        get{
            return arr[a]
        }
        set{
            guard a>=0 && a<arr.count else{
                arr.append(newValue)
                return
            }
            arr[a] = newValue
        }
    }
}

var arrs = Arrays(arr: [])
arrs[1] = "111"
arrs[2] = "222"
print(arrs)

执行结果：
3
AAAS的内容为1
Arrays(arr: ["111", "222"])
```

> 上面分别创建两个下标，Test创建一个只读下标，Arrays创建了一个读写下标



## 类型下标

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
    static subscript(n: Int) -> Planet {
        return Planet(rawValue: n)!
    }
}
let mars = Planet[4]
print(mars)
```

在下标前添加static，这样下标就变成类型下标，需要通过类名访问。

使用class替代static，实现子类可以重写父类的下标	