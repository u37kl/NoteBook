# 抛出异常

>throw：创建并抛出具体的异常。
>
>throws：声明函数将会抛出异常，告诉编译器。
>
>Error：所有错误必须实现Error协议



```swift
enum PersonError:Error {
    case noName
    case noAge
    case noAddress
    case description(a:Int, b:Int)
}

class Person{
    var name:String = ""
    var age:Int = 0
    var address:String = ""
    static func createPerson(name:String?, age:Int?, address:String?) throws -> Person {
        if name == nil {
            throw PersonError.noName
        }else if age == nil{
            throw PersonError.noAge
        }else if address == nil{
            throw PersonError.noAddress
        }else if age! < 18{
            throw PersonError.description(a: 12, b: 12)
        }
        
        let p = Person()
        p.name = name!
        p.address = address!
        p.age = age!
        
        return p
    }
}

代码分析：
throws添加在参数列表后，->前面。
```





# 截获与处理异常

错误的处理一般只有3种情况：

> 1. 使用do-catch对错误进行处理
> 2. 使用`try?`，将错误转换成nil，这样就不需要处理
> 3. 直接使用try，将错误抛给上层。



## try、try?、try!

> swift提供了3种截获异常的方式：
>
> 1. try：捕获函数或者方法抛出的异常，等待do-catch处理
> 2. Try?：捕获函数或者方法抛出的异常，并将其转换为可选类型`nil`。
> 3. try!：告诉编译器需要捕获的方法或者函数不会抛出异常，如果抛出异常，程序崩溃



使用try?捕获异常时，函数或者方法的返回值都会转换成可选类型，无论是否出现异常。

## do-catch

使用do-catch来处理异常代码，catch分支来指定处理某个异常错误，`必须保证将函数或者方法抛出的异常全部处理`，否则编译器报错。

> 需要注意的是do-catch中代码执行出现了错误，则`do代码块`中的代码不会再继续执行，而是直接跳到相应的catch分支中处理异常。。

```swift
func test(){
    do{
			let p = try Person.createPerson(name: "aaa", age: 16, address: "ssds")
        
    }catch PersonError.noName{
        print("PersonError.noName")
    }catch PersonError.noAge{
        print("PersonError.noAge")
    }catch PersonError.noAddress{
        print("PersonError.noAddress")
    }catch PersonError.description(let a, let b){
        print("PersonError.description --- \(a) === \(b)")
    }catch{
        
    }
  print("处理完错误后，执行的第一句话");
}

func test3(){
    let p:Person? = try? Person.createPerson(name: "aaa", age: 20, address: "bbb")
  	let p1:Person = try! Person.createPerson(name: "aaa", age: 20, address: "bbb") 
}

test()

代码执行结果：
PersonError.description --- 12 === 12
处理完错误后，执行的第一句话

代码分析：

try? 返回的是捕获的函数或者方法的返回值类型对应的可选类型。
try! 返回的是捕获的函数或者方法的返回值类型。
```



## 错误传递

如果当前方法调用会抛出异常的方法时，有两种解决方式

> 1. 方法内部使用do-catch语句，处理错误
> 2. 将错误向上抛，有上层解决，这就是`错误传递`

错误传递时，如果上层不去解决时，必须将错误再向上层抛，知道抛给系统，然后程序崩溃。

```swift
func test1() throws{
    let p = try Person.createPerson(name: "aaa", age: 16, address: "ssds")
}


func test2(){
    do{
      try test1()
        
    }catch PersonError.noName{
        print("PersonError.noName")
    }catch PersonError.noAge{
        print("PersonError.noAge")
    }catch PersonError.noAddress{
        print("PersonError.noAddress")
    }catch PersonError.description(let a, let b){
        print("PersonError.description --- \(a) === \(b)")
    }catch{
        
    }
    
}

test2()
```



# defer代码块



代码块返回之前，必须执行defer代码块，无论是否发生异常。一般用在发生异常时，需要完成的操作，例如文件读写操作错误时，释放文件读写对象。

> 使用注意：
>
> 1. defer代码块必须放在`return`之前，否则不会执行。即程序执行到该语句才能触发。
> 2. 运行到defer代码块时，不会执行defer代码块，直到defer所处的代码块执行完毕退出之前时，才会执行defer代码块。

```swift
func test4(){
 {	
 	   defer {
        print("aaaaa")
    }
   print("cccccc")
 }
    
    print("bbbbb")
}

func test5(){
    do {
        defer {
            print("dddd")
        }
        print("eeee")
    }
    
    print("ffff")
}

test4()

执行结果：
bbbbb
aaaaa
eeee
dddd
ffff
```





