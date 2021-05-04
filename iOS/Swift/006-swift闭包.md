# 闭包

## 闭包的用途与格式

闭包3种用途：

1. 作为一个常量或者变量值。
2. 作为函数或者方法的参数。
3. 作为函数或者方法的返回值。

闭包的出现大大的扩展了函数的功能，以前只能传入和返回数据，有了闭包后，可以向函数或者方法中传入和返回代码块了。



闭包的基本结构

```swift
// 结构
{
  [值捕获列表] (形式参数名: 数据类型, [形式参数名: 数据类型]) -> 返回值类型 in
  
  // 自定义代码
  
  return
}


// 案例
var block:(String, Int) -> String = {
  (str: String, num: Int) -> String in
  // 自定义代码
  return ""
}
```



## swift提供的四种闭包



### 普通闭包

上面介绍的闭包格式就是普通闭包，根据swift的类型推断功能可以对闭包进行简写

```swift
var block:([String]) -> String = { (nums:[String]) -> String in
    var s = nums[0]
    for item in 1..<nums.count {
        s = nums[item] + s
    }
    return s
}

// 省略参数名和返回值
block = {
  var s = $0[0]
    for item in 1..<$0.count {
        s.append($0[item])
    }
    return s
}

// 省略参数名、返回值和return，如果闭包中只有一个表达式时，可以省略return
var block:(Int, Int) -> Bool{ $0 >$1 }
var block:(Int, Int) -> Bool{ > }

```



### 尾随闭包

闭包经常使用的场景是，在调用函数时设置闭包，将闭包体放在函数参数列表中，降低可读性和编写的复杂性，因此swift提供了尾随闭包。

当函数或者方法的参数列表中`最后一个参数`是闭包时，可以将`闭包放在小括号后面`，提高可读性。



尾随闭包的两种情况：

1. 当函数或者方法的形式参数有多个时，将闭包体放在小括号后面。
2. 当函数或者方法只有一个形式参数并且是闭包时，可以将小括号省略。

```swift
// 函数或者方法的最后一个参数是闭包，调用时将闭包体放在括号外
    func block1(arr:[String], block:([String])->String){
        var str = block(arr)
    }

		block1(arr: ["1", "2", "3", "4", "5"]) { (arr: [String]) -> String in
         var str = arr[0]
         for i in 1..<arr.count{
            str.append(contentsOf: arr[i])
         }
         return str
    }
    
//  函数或者方法只有一个参数，调用时省略小括号
    func block2(block:([String])->String){
        var arr = ["1", "2", "3", "4", "5"]
        block(arr)
    }

         
    block2 { (arr: [String]) -> String in
         var str = arr[0]
         for i in 1..<arr.count{
           str.append(contentsOf: arr[i])
         }
         return str
     }
```



### 逃逸闭包

逃逸闭包：闭包作为参数传递给函数或者方法，但是闭包的执行在函数或者方法执行完成之后。

使用`@escaping`关键字来声明函数或者方法中的闭包是逃逸闭包

```swift
// 闭包数组
var blocks = [(String) -> String]()
// test3中声明了一个逃逸闭包
func test3(block: @escaping (String) -> String){
    blocks.append(block)
}
// 调用test3时，传入一个闭包
test3 { (str) -> String in
    return str
}

var s = blocks.count > 0 ? blocks[0]("12345") : ""
print(s)
```



### 自动闭包

自动闭包：函数或者方法对通过参数传入的`一句代码`自动封装成闭包。

使用`@autoclosure`关键字来声明函数或者方法中的闭包是自动闭包。

自动闭包特点：

1. 闭包体中只能是`一句代码`，因为是由函数自动封装成闭包的。
2. 该闭包不接收任何参数，可以有返回值。

```swift
// test4声明了一个自动闭包自动闭包
func test4(block: @autoclosure () -> String){
    print(block())
}
// 将"1234561"作为闭包体，封装成一个闭包
test4(block: "1234561")

var arrStr = ["111", "2222", "3333"]
// test5声明了一个自动闭包自动闭包
func test5(block: @autoclosure () -> String) {
    print("\(block())")
}
test5(block: arrStr.remove(at: 0))
print(arrStr)
```

## 闭包的值捕获

见文章`022-swift内存安全`

