

# 子类与父类

swift中默认所有自定义的类都是基类，不继承任何类。

如果想要继承某个类需要在创建类时设置该类的父类

## 继承格式

```swift
class subClass : SuperClass{

}
```



# 重写

> swift对重写的检查：swift会对重写的属性和方法进行格式检查，检查父类是否有匹配的同名同类型的属性和方法，匹配失败则编译报错。
>
> swift中使用`override`关键字，表示子类在重写父类中的某个成员。



## 重写方法

> 重写方法时，swift会检查父类是否有同名同类型的方法，SubSclass重写的方法名为test，类型为()->String

```swift
class SuperClass{
    func test() -> String{
        return ""
    }
}

class SubClass : SuperClass{
  
    override func test()->String{
        return "aaa"
    }
}
```





## 重写属性

> 重写属性规则：
>
> 1. 对于父类是存储属性情况，只支持将`父类的存储属性，重写为可读可写计算型属性`。
> 2. 对于父类是计算属性时，如果父类是`只读计算属性`，则子类可以重写为`只读计算存储`和`可读可写计算属性`，如果父类是`可读可写计算属性`，则子类只能重写为`可读可写计算属性`。
> 3. 子类不能将父类的计算属性重写为存储属性。
>
> 转化规则：
>
> 存储属性--> 可读可写计算属性 
>
> 只读属性 --> 可读可写属性/只读属性
>
> 可读可写计算属性 --> 可读可写计算属性



> 重写属性目的：
>
> 1. 给父类中的存储属性添加属性监听。
> 2. 修改父类中的计算属性

```swift
class SuperClass{
    var a:Int = 1
  
    var b:Int{
        get{
            return a
        }

    }
    
    var c:Int{
        get{
            return 3
        }
        
        set{
            a = newValue
        }
    }
    
    var d:Int{
        get{
            return 3
        }
        
        set{
            a = newValue
        }
    }
}

class SubClass : SuperClass{
    // 父类是存储属性，子类重写为计算型属性
    override var a: Int{
        get{
            print("SubClass类的getter -- a")
            return super.a
        }
        
        set{
            print("SubClass类的setter -- a = \(newValue)")
            super.a = newValue
        }
    }
    
    // 父类是只读属性，子类可读可写属性
    override var b: Int{
        get{
            return 12
        }
        
        set{
            a = newValue
        }
    }
    
    // error：父类可读可写计算属性，不能重写为只读计算属性
    override var c: Int{
        get{
            return 12
        }
    }
    
    // 父类时可读可写属性时，子类不能只重写setter方法，还需要重写getter方法
    override var d: Int{
        set{
            super.a = newValue
        }
        
        get{
          // 如果不想重写父类的getter时，调用super
            return super.d
        }
    }
    
}

上面代码：说明了子类在重写父类的属性时应该注意的点，尤其是重写属性c是，父类可读可写，子类不能重写为只读属性。
```

> 子类重写父类的计算型实现时，只实现setter方法可以吗？
>
> 如果你提供了一个setter作为属性重写的一部分，你也就必须为重写提供一个getter。如果你不想在重写getter时修改继承属性的值，那么你可以简单通过从getter返回 super.someProperty 来传递继承的值， someProperty 就是你重写的那个属性的名字。



### 重写属性观察器

> 属性观察器重写规则：
>
> 1. 子类不能同时重写`属性观察器`和`属性的getter、setter`。
> 2. 父类如果是只读计算属性或者常量时，子类不能重写属性观察器，数据不会改变，重写没有意义。
> 3. 在子类中可以利用属性重写机制给父类计算型属性添加属性监听。



```swift
class SuperClass{
    var a:Int = 1{
        willSet{
            print("SuperClass --- a --- willSet")
        }
        
        didSet{
            print("SuperClass --- a --- didSet")
        }
    }
    var c:Int{
        get{
            return 3
        }
        
        set{
            a = newValue
        }
    }
}

class SubClass : SuperClass{
    // 父类是存储属性，子类重写为计算型属性
    override var a: Int{
        willSet{
            print("SubClass --- a --- willSet")
        }
        
        didSet{
            print("SubClass --- a --- didSet")
        }
    }
    
    override var c: Int{
        willSet{
            print("SubClass --- c --- willSet")
        }
        
        didSet{
            print("SubClass --- c --- didSet")
        }
        
    }
    
}

var sub = SubClass()
sub.a = 12
sub.c = 13

执行结果：
SubClass --- a --- willSet
SuperClass --- a --- willSet
SuperClass --- a --- didSet
SubClass --- a --- didSet
SubClass --- c --- willSet
SubClass --- a --- willSet
SuperClass --- a --- willSet
SuperClass --- a --- didSet
SubClass --- a --- didSet
SubClass --- c --- didSet

分析结果：
	重写属性监听后，还是会触发父类的属性监听
```



## super关键字

​		super表示父类，在子类的属性、方法和下标中，可以通过super访问父类的属性、方法和下标

> super[index]：访问父类的下标
>
> super.varName：访问父类的属性
>
> super.function()：访问父类的方法



# final关键字

​		final关键字表示终结重写行为。

> final class：终结当前类的重写行为，即不能`派生子类`
>
> final var：子类不能重写该属性
>
> final func：子类不能重写该方法
>
> final subscript：子类不同重写该下标