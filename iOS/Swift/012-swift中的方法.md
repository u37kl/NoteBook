# 方法定义

与函数结构一样

# 方法使用范围

​		OC中只有类能定义方法，但在swift中，类、结构体和枚举都可以定义方法，不仅可以定义实例方法还可以定义类型方法(相当于类方法)

```swift
struct Test{
    var a:Int
    var b:Int
    
    func description(){
        print("\(a) --- \(b)")
    }
}

var t = Test(a:1, b:2)
t.description()

enum Fruit{
    case banana
    case apple
    case orange
    
    func description(_ fruitName: Fruit){
        switch fruitName {
        case .banana:
            print("这是香蕉")
        case .apple:
            print("这是苹果")
        case .orange:
            print("这是橘子")
        }
    }
}

var f = Fruit.apple
f.description(f)

打印结果：
1 --- 2
这是苹果
```



# self关键字

`self代表当前调用方法的实例对象`

>`self`一般两个作用：
>
>1. 在方法中调用该数据类型(类、结构体、枚举)的其他方法或者属性
>2. 当前方法的形参名称与属性名相同时，使用self做区分。



# mutating方法

​		在结构体和枚举的实例调用方法时，无法修改属性，因此使用mutating方法对实例属性进行修改。

​		原理：其实是`重新创建`一个`新的该类型的实例`，并赋值给保存原来实例的变量或者常量。

```swift
struct Test{
    var a:Int
    var b:Int
    
    mutating func setTest(_ a:Int, _ b:Int){
        self.a = a
        self.b = b
    }
    
    func description(){
        print("\(a) --- \(b)")
    }
}

struct Test1 {
    var a:Int
    var b:Int
    
    mutating func setTest(_ a:Int, _ b:Int){
        self = Test1(a: a, b: b)
    }
    
    func description(){
        print("\(a) --- \(b)")
    }
}


var t = Test(a:1, b:2)
t.description()
t.setTest(5, 11)
t.description()

// 三太开关
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}

var s = TriStateSwitch.off
print(s)
s.next()
print(s)
```

>上面提供了两种mutating方式：
>
>1. 修改指定的属性，像Test类中，self.a = a
>2. 创建一个新的实例，并赋值给self，像Test1类中，self = Test1(a: a, b: b)
>
>两种方式原理一样，只是写法不同

# 类型方法



> 1. Swift允许`类`、`结构体`和`枚举`创建类型方法。
>
> 2. 使用`static`关键字定义类型方法，对于类，如果允许当前方法被子类重写，则使用`class`代替`	static`
>
> 3. 在类型方法的`self`指向当前类对象。
> 4. 在类型方法中可以直接使用属性名或者类型方法，不需要class名.方法名

```swift
/* 关卡追踪者，
	 使用类型属性highestUnlockedLevel，保存当前系统解锁的最高关卡。
	 使用实例属性currentLevel，保存当前玩家正在游玩的关卡
*/
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1
    
    // 解锁关卡
    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }
    
  // 判断关卡是否解锁
    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }
    
  // 设置想要游玩的关卡，并保证不会设置成未解锁的关卡
    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}

// 玩家类
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}

var player = Player(name: "Argyrios")
// 解锁下一个关卡
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")

if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 has not yet been unlocked")
}

打印结果
highest unlocked level is now 2
level 6 has not yet been unlocked
```

>` @discardableResult`是swift语言特性：把这个特性用在函数或方法的声明中，当调用一个有返回值的函数或者方法却没有使用返回值时，编译器不会产生警告。



# 方法识别

与函数识别一样

