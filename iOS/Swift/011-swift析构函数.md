# 析构函数

使用`deinit`创建析构函数，用来在实例对象在释放前，做一些释放内存的操作。

`由于析构函数执行完成，实例对象才能释放`，因此在析构函数中可以实例对象中的属性还没有被释放，可以访问实例对象中的所有属性和方法。



## 析构函数结构

```swift
deinit{
  释放内存空间代码
}
```



# 析构函数实例

定义一个玩家类和银行类，银行类管理所有玩家金钱，当玩家对象下线时，需要将钱退还给银行，因此用到了析构函数。

```swift
import Foundation

class Bank{
    static var money:Int = 10_1000
    static func expensesMoney(money:Int) -> Int{
        if self.money >= money {
            self.money -= money
            return money
        }else{
            return 0
        }
    }
    
    static func receiveMoney(money:Int){
        self.money += money
    }
}

class Player{
    var money:Int = 0
    
    func borrowMoney(money:Int){
        self.money += Bank.expensesMoney(money: money)
    }
    
    func payBack(){
        print("还给银行\(self.money)钱")
        Bank.receiveMoney(money: self.money)
        self.money = 0
    }
    
    deinit {
        print("调用析构函数")
        payBack()
    }
}


var player:Player? = Player()
player?.borrowMoney(money:1000)
// 当Player实例引用计数器为0时，系统会释放该实例，在此之前调用析构函数
player = nil

打印结果：
调用析构函数
还给银行1000钱

```

