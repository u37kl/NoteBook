# 函数定义结构

函数结构

```swift
func 函数名(实际参数标签 形式参数名: 数据类型, [实际参数标签 形式参数名: 数据类型]..) -> 返回值类型{
  函数体
}
```



函数实例

```swift
func add(firstP a:Int, secondP b:Int) ->Int{
  return a+b
}
```



## 各种函数写法

### 无参函数

```swift
func test() -> String{
  return "abc"
}
```



### 无返回值函数

```swift
func test(a:Int){
  print("a= \(a)")
}
```



### 多返回值

```swift
func netWorking(url:String) -> (Int, String){
  return (404, "不存在当前请求的资源")
}
```



### 隐式返回

如果只有函数体只有单一表达式时，可以直接不写`return`

```swift
func add(a:Int, b:Int) -> Int{
  a+b
}
```



## 参数列表介绍

调用函数时，传入参数会进行`值拷贝`，使用`常量保存`，因此即使在函数外部时变量，传入到函数中时，就是常量。

### 形式参数和实际参数标签

swift的函数参数有两个参数名，例如`func test(param a:Int)`，param就是实际参数标签，而a就是形式参数，前者是用来外部函数调用使用，后者是在函数体内部使用。

```swift
func test(param a:Int){
  print(a)
}

test(param: 12) // 使用实际参数标签传入参数。
```



默认实际参数标签和忽略实际参数标签，前者不设置实际参数标签使用形式参数名称当作实际参数标签，后者使用`_`表示忽略实际参数标签。`实际参数标签作为函数识别使用`

```swift
func test(a:Int) -> Int{
  return a+3
}

test(a:12)

func test(_ a:Int) -> Int{
  return a+3
}
test(12)

代码分析：
从上面代码可以看出方法调用差别。
```



### 可变参数

一个函数只能有一个可变参数。

```swift
func test(arrs:Int..., b:Int) -> Int{
    var arr = [Int]()
    for (index, item) in arrs.enumerated() {
        arr[index] = item + b
    }
    return arr.reduce(0){ (result:Int, num:Int) ->Int in
        return result + num
    }
}

test(arrs: 1,2,3,4,5, b: 2)
```



### 输入输出参数

由于函数的参数默认都是常量，无法进行修改，使用`inout`修饰参数，这样可以修改函数外部的变量，调用时在变量前面加`&`。

> 原理：使用变量保存外部传入的数据，当函数调用结束时，将其赋值会外部的变量中。
>
> 使用输入输出参数可以实现不用函数返回值，返回函数执行结果。

```swift
var a = 12
func test(a: inout Int){
  a = 5
}
test(&a)
print(a)
```



## 函数类型

swift提供了函数类型，可以将函数作为一种数据类型，在函数中传递。

```swift
// 函数作为数据类型
func test(a:Int)->String{
  return String(a)
}

var a:(Int)->(String) = test

// 函数作为参数
func test(a:(Int) -> Void){
  
}

test{ (a:Int) in
	print(a)    
}

// 函数作为返回值
func test() -> (Int)->Void{
    return {(a:Int) in
        print("aaa")
    }
}

test()(12)

```



# 函数重载

> swift通过比较方法名、参数列表和返回值类型来判断方法是否重复定义。
>
> 判断函数是否重复定义的方式：
>
> 1. 函数名是否相同
> 2. 返回值类型是否相同
> 3. 参数列表中参数个数是否相同
> 4. 参数列表中相对应位置参数的数据类型是否相同
> 5. 参数列表中相对应位置参数的`实际参数标签`是否相同，比较的是实际参数标签，而不是形式参数名
>
> 如果上述5项都相同时，则函数定义重复。



```swift
class SuperClass{
    var a: Int = 2
    var b: Int = 1
    let c:String = ""
    
    func test(a:Int, b:Int, c:String){}
    // 方法名不同
    func my(a:Int, b:Int, c:String){}
    // 参数列表顺序不同
    func test(b:Int, a:Int, c:String){}
    // 参数列表个数不同
    func test(a:Int, b:Int){}
    // 对应位置的实际参数标签名不同
    func test(s:Int, b:Int, c:String){}
    // 返回值不同
    func test(a:Int, b:Int, c:String)->Bool{ return true}   
}
```



测试题：

> 1. `init?()`与`init()`是同一个函数吗
> 2. 使用关键字是否可以让编译器认为是不同函数，例如`convenience init()` 与 `init()`
>
> 编译器判断是否是同一个方法，使用的是上面的规则，因此这两个问题中定义的3个方法都是同一个方法。



答案：

> init()、init?()和convenience init()都是同一个参数，编译器会报函数重复定义的错误，因为按照5项判定规则，它们都一样。