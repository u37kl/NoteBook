# 数组

## 数组创建

```swift
/*
 * 创建数组的三种方式
 */
let lArr1 = [1,2,3,4,5]
let lArr2 = [Int](repeating: 0, count: 5)
let lArr3 = Array<String>(repeating: "", count: 5)

/*
 * 数组的数据类型
 */
let lArr4:[String] = ["111","222"]
let lArr5:[Any] = [1,"23", 1.24]
let lArr6:[NSObject] = [1 as NSNumber, "AD" as NSObject]

/*
 * 可变数组
 */
let lArr = [1,2,3,4,5] // 不可变数组
var vArr = [1,2,3,4,5] // 可变数组

```

## 增、删、改

```swift
var lArr1 = [1,2,3,4,5]
// 数组长度
let length = lArr1.count

// 修改数组中元素
lArr1[1] = 12

// 删除从下标1开始3个数据，并从下标1中插入两个数据
lArr1[1...3] = [8, 20]

// 追加元素
lArr1.append(6)
// 插入元素
lArr1.insert(0, at: 0)

// 同类型数组合并
lArr1 += [7,8,9] // 同类型

// 删除c数组
lArr1.remove(at: 0)
lArr1.removeLast(2)

// 查询数组是否含有某个元素
lArr1.contains(13)
```



## 遍历

```swift
for temp in lArr {
    print(temp, terminator: " ")
}
print()

for (index, value) in lArr.enumerated(){
    print("\(index) : \(value)", terminator: ", ")
}
print()
```



## Map、flatMap、filter、reduce

>map：遍历数组，并对其中元素进行操作，返回新数组
>
>flatMap：遍历数组，并对其中元素进行操作，返回新`降维`数组。
>
>filter：遍历数组，并按条件过滤不符合的元素，生成新数组
>
>reduce：将数组中的元素合并，返回一个元素类型数组，例如对数组求和。



```swift
import Foundation
let lArr1 = [1,2,3,4,5]
var lArr2 = [[1,2,3,],[4,5,6], [7,8,9]]


// 每个元素+1
var arrs = lArr1.map { (num: Int) -> Int in
    return num + 1
}
print(arrs)

// 对二维数组中每个元素+1
let arr1 = lArr2.map(){
    (nums: [Int]) -> [Int] in
    nums.map(){
        (num: Int) -> Int in
        return num + 1
    }
}
print(arr1)

// 将二维数组转成一维数组，并对每个数组+1
let arr2 = lArr2.flatMap(){
    (nums: [Int]) -> [Int] in
    nums.map(){
        (num: Int) -> Int in
        return num + 1
    }
}
print(arr2)

// 删除数组中<2的元素
let arr3 = lArr1.filter { (num:Int) -> Bool in
    return num > 2 ? true : false
}
print(arr3)

// 查看数组元素总数
let sum = lArr1.reduce(0) { (result:Int, item:Int) -> Int in
    return result + item
}
print(sum)


```



# Set集合

## 创建

```swift
var vSet1 = Set<String>()
var vSet2:Set<String> = ["1", "2", "3"]
var vSet3 = ["a", "b", "c"] // vSet2创建的简化形式，赋值数组必须数据类型一致，这样才能利用类型推断
```



## Set集合操作

> `insert方法`：返回一个元组(inserted: Bool, memberAfterInsert: Set<Element>.Element)，是否添加成功，和添加的元素。
>
> `remove方法`：返回需要删除的元素，如果元素不存在，则返回nil



```swift
var vSet2:Set<String> = ["1", "2", "3"]
// Set集合元素个数
vSet2.count
// 判断set集合是否为空
vSet2.isEmpty
// 判断是否包含某个元素
vSet2.contains("1")
// 添加元素
vSet2.insert("1")
// 删除元素
var rem = s.remove("4")

// 遍历元素
for item in vSet2 {
    print(item)
}

// 过滤Set集合元素
let fruits: Set = ["apple", "pear", "orange"]
//过滤出以“e”结尾的所有元素
let result = fruits.filter({ $0.hasSuffix("e") })
print(result)
```



## Set集合的集合操作(交集、并集、差集)

```swift
 * union：并集
 * intersection：交集
 * subtracting：差集
 * symmetricDifference：并集 - 交集
 */

let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

let set1 = oddDigits.union(evenDigits).sorted() // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
let set2 = oddDigits.intersection(evenDigits).sorted() // []
let set3 = oddDigits.subtracting(singleDigitPrimeNumbers).sorted() // [1, 9]
let set4 = oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted() // [1, 2, 9]
```



## Set集合包含与被包含

```swift
/*
 * isSubset：set5是set6的子集
 * isSuperset：set5是set6的父集
 * isStrictSubset： set5是否是set6的子集，并且两个集合不相等
 * isStrictSuperset： set5是否是set6的父集，并且两个集合不相等
 * isDisjoint ：两个集合是否没有交集
 */

let set5: Set = [1, 3, 5]
let set6: Set = [1, 3, 5, 7, 9]
let set7: Set = [1, 2, 3]

set5 == set6 // false
set5.isSubset(of: set6) // true
set5.isSuperset(of: set6) // false
set5.isStrictSubset(of: set6) // true
set5.isStrictSuperset(of: set6) // false
set5.isDisjoint(with: set7) // false
```



## 自定义类型添加到Set集合

> Set集合通过哈希表来保存元素，因此所有想要保存到Set集合的元素必须要支持哈希化，实现方式：
>
> 1. 遵循Hashable协议，并实现hashValue方法
> 2. 遵循Equatable协议，并实现`==`方法



```swift
//自定义对象
struct User {
    let name: String
    let age: Int
}

//自定义对象需要符合Hashable协议
extension User: Hashable {
    var hashValue: Int { return name.hashValue ^ age.hashValue }
}

//由于Hashable协议又符合Equatable协议，所以还要提供一个"是否相等"运算符(==)的实现
func ==(lhs: User, rhs: User) -> Bool {
    return lhs.name == rhs.name && lhs.age == rhs.age
}

var u1 = User(name: "Tom", age: 12)
var u2 = User(name: "John", age: 18)
var u3 = User(name: "Patter", age: 21)

var s:Set = [u1, u2, u3]
```



# 字典

> 字典保存数据时，要求key可以哈希化，即key的数据类型必须遵循`Hashable`和`Equatable`两个元素。
>
> 从字典中取出的元素都是可选类型，因为存在查询不到的数据



## 创建

```swift
var vDict1 = [1 : "a", 2 : "b", 3 : "c"]
var vDict2 = [String : String]()
var vDict3:[Int:String] = Dictionary()
```



## 字典操作

```swift
var vDict1 = [1 : "a", 2 : "b", 3 : "c"]
// 查看字典元素个数
vDict1.count
// 查看字典是否为空
vDict1.isEmpty
// 根据key查询，查询不到返回nil
let v2:String? = vDict1[4]

// 根据key设置值，如果key不存在则添加，存在则修改
vDict1[4] = "d"
// 删除元素
vDict1[4] = nil

// 将字典中的key或者value转换成数组
var arrs1 = Array(vDict1.keys)
var arrs2 = [String](vDict1.values)

// 遍历字典
for (key, value) in vDict1 {
    print("\(key) : " + value, separator: "", terminator: " ")
    
}

// 读取字典中的所有key，并进行排序
for key in vDict1.keys.sorted() {
    print("\(key) : " + vDict1[key]!, terminator: " ")
}

for value in vDict1.values {
    print(value, terminator: " ")
}
```



