# KVC

​	KVC又叫做键值编码，不借助类提供的接口(setter和getter)，访问类中的属性和成员变量，即使是私有属性和成员变量，也可以通过KVC进行读写，大大提高编程灵活性，但也破坏了系统封装性。

​	KVC是借助runtime，来获取类中信息，从而进行数据的读写。NSObject类的一个分类中定义了KVC的功能，因此只有继承NSObject的类才可以使用KVC。



## KVC使用方式

​	KVC可以访问类中的基本数据类型、结构体、非集合对象和集合对象。类的结构并不是只有一层，对于属性或者成员变量也是一个类的多层类，KVC可以实现读写。

### forKey

​	使用forKey只能访问类中单层属性。

```objective-c
- (void)setValue:(nullable id)value forKey:(NSString *)key;
- (nullable id)valueForKey:(NSString *)key;
```



### forKeyPath

​	使用forKey可以访问类中深层属性，即只有类的属性或者成员变量是一个类，就可以通过KVC访问这个类中的属性。

```objective-c
- (nullable id)valueForKeyPath:(NSString *)keyPath;
- (void)setValue:(nullable id)value forKeyPath:(NSString *)keyPath;
```



### 基本类型和结构体的KVC

​	由于KVC是基于NSObject的，因此使用KVC读写基本类型或者结构体时，需要将其转换成NSNumber或者NSValue。

```objective-c
ZPPerson *p = [ZPPerosn new];
// 对于基本类型读写，需要转换成NSNumber。
[p servalue:@(12) forKey:@"age"];
NSNumber *num = [p valueForKey:@"age"];

//  对于结构体类型读写，需要转换成NSValue。
typedef struct {
    int chinese;
    int math;
    int english;
} GradeStruct; // 成绩

GradeStruct gds = {92, 100, 34};
NSValue *valueOfGradeStruct = [NSValue value:&gds withObjCType:@encode(GradeStruct)];
[p setValue:valueOfGradeStruct forKey:@"gds"];
GradeStruct gds2;
[[p valueForKey:@"gds"] getValue:&gds2];
```



### 集合操作

#### 数组

​	对于数组的KVC操作有两种：

	1. 数组的聚集函数，sum、avg、min、max计算元素属性的总和、平均值、最小、最大
 	2. 属性为数组，元素为自定义对象，读取数组元素中的某个属性。
 	3. @unionOfObjects、@distinctUnionOfObjects，前者并集，后者是去重。

```objective-c
@interface ZPPerson5 : NSObject
@property (nonatomic, copy) NSString *name;
@property (nonatomic, assign) NSInteger age;
@property (nonatomic, strong) NSMutableArray<ZPPerson6 *> *friends;
@end

NSLog(@"%@",[p valueForKeyPath:@"friends.@sum.age"]);
NSLog(@"%@",[p valueForKeyPath:@"friends.@avg.age"]);
NSLog(@"%@",[p valueForKeyPath:@"friends.@min.age"]);
NSLog(@"%@",[p valueForKeyPath:@"friends.@max.age"]);
NSArray *arr = [p mutableArrayValueForKeyPath:@"friends.age"];
NSLog(@"%@",[p valueForKeyPath:@"friends.@unionOfObjects.age"]); // 获取所有朋友的年龄
NSLog(@"%@",[p valueForKeyPath:@"friends.@distinctUnionOfObjects.age"]); // 获取所有朋友的年龄并去重。
```



#### 字典

​	KVC提供了批量的读写数据的方式，字典与模型的相互转换，支持属性和成员变量，支持基本类型、结构体、OC类型。

```objective-c
// ZPPerson5.h
@interface ZPPerson5 : NSObject
@property (nonatomic, copy) NSString *name;
@property (nonatomic, assign) NSInteger age;
@property (nonatomic, strong) ZPPerson5 *father;
@property (nonatomic, strong) NSMutableArray *friends;
@end

// ZPPerson5.m
@interface ZPPerson5 ()
{
    BOOL _sex;
    NSString * test1;
    NSString * _test2;
    NSString * isTest3;
    NSString * _isTest4;
    NSMutableArray *_arrms;
    NSSet *_sets;
    
    GradeStruct _gds ;
    
}
@end

// KVCy提供了两个关于字典的方法，实现字典与模型的转换。
ZPPerson6 * p = [ZPPerson new];
// 模型转字典
NSDictionary *dictOfPerson = [p dictionaryWithValuesForKeys:@[@"age", @"name", @"father", @"test1", @"_test2", @"isTest3", @"_isTest4"]];

// 字典转模型
NSDictionary *dict = @{@"name":@"王司徒", @"age":@(76), @"test1":@"ggggg", @"_test2":@"jjjjj", @"isTest3":@"kkkkk", @"_isTest4":@"vvvvv"};
[p setValuesForKeysWithDictionary:dict];
```





## KVC搜索key的步骤

### setValue:forKey:和setValue:forKeyPath: 

> 1、查询是否有set<key>:方法，有则调用，KVC结束。
> 2、系统调用该类中实现的accessInstanceVariablesDirectly方法，是否还要继续搜索，YES则继续向下走，NO则直接跳到第4步。
> 3、按照_key、_isKey、key、isKey顺序查找是否相关的成员变量，找到则KVC结束，没有找到进入第4步。
> 4、上面步骤走完没有找到，调用setValue:forUndefinedKey:方法，如果不实现程序崩溃。



### valueForKey:和valueForKeyPath:

>1、查询是否有get<Key>,<key>,is<Key>方法，有则调用。
>2、查询是否有countOf<key>、objectIn<key>AtIndex、<key>AtIndexes方法，如果countOf<Key>方法和另外两个方法中的一个被找到，那么就会返回一个可以响应NSArray所有方法的代理集合(它是NSKeyValueArray。
>3、查找countOf<Key>，enumeratorOf<Key>,memberOf<Key>格式的方法。如果这三个方法都找到，那么就返回一个可以响应NSSet所的方法的代理集合。
>4、系统调用+accessInstanceVariablesDirectly方法，是否还要继续搜索，返回NO，则直接进入第6步。
>5、按照_<key>,_is<Key>,<key>,is<Key>的顺序搜索成员变量名。
>6、调用valueForUndefinedKey:方法，如果没有实现则系统崩溃。

总结：

1、从上面的步骤可以看出，KVC首先搜索setter和getter，因此即使没有定义成员变量只是实现了setter和getter方法，可以用使用KVC。

2、KVC只会搜索相应名称的setter、getter或者成员变量，因此像分类中定义属性时，property_list有key对应属性，而ivar_list没有对应成员变量，又没有实现相应setter和getter方法，使用KVC会导致找不到key而程序崩溃。



### KeyPath的搜索步骤

​	

```objective-c
// ZPPerson.h
@interface ZPPerson5 : NSObject
@property (nonatomic, copy) NSString *name;
@property (nonatomic, assign) NSInteger age;
@property (nonatomic, strong) ZPPerson5 *father;
@property (nonatomic, strong) NSMutableArray *friends;
@end

// ZPPerson5.m
@interface ZPPerson5 ()
{
    ZPIDCard * _card;
}
@end
@implementation ZPPerson5
- (instancetype)init
{
    self = [super init];
    if (self) {
        _card = [ZPIDCard new];
        return self;
    }
    return nil;
}
// 当使用KVC查找到一定程度时，系统会调用该方法，询问是否还需要向下查找。
	+ (BOOL)accessInstanceVariablesDirectly
	{
    NSLog(@"ZPPerson5，查找到一定程度，是否还要继续搜索");
    return NO;
	}
@end
 
// ZPCard
@interface ZPIDCard : NSObject

@end
  
@interface ZPIDCard()
{
    NSString * _idCard;
}
@end

@implementation ZPIDCard

+(BOOL)accessInstanceVariablesDirectly{
  NSLog(@"ZPIDCard，查找到一定程度，是否还要继续搜索");
    return NO;
}
@end
  
  
// main
ZPPerson5 *p = [ZPPerson5 new];
[p setValue:@"asdfsfd" forKeyPath:@"idCard._idCard"];
```



> 分别控制ZPPerson5和ZPIDCard类中的+accessInstanceVariablesDirectly方法。
>
> 1、ZPPerson5返回NO，ZPIDCard无所谓，终端打印ZPPerson5类中acess..方法，系统崩溃。
>
> 2、ZPPerson5返回YES，ZPIDCard返回NO，终端打印ZPIDCard类中acess..方法，系统崩溃。

由此可以得出，KeyPath其实还是与key的搜索步骤一样，例如"idCard._idCard"，查找ZPPerson5类中是否有“idCard”对应的setter或者成员变量，如果找到进入该成员变量的类中(ZPIDCard)，查找ZPIDCard中"__idcard"对应的setter或者成员变量。



## KVC异常处理

KVC中的两种异常：

> 1、给基本数据类型或者结构体设置nil。
>
> 2、找不到key。

### 设置nil

​	iOS允许使用KVC给OC对象设置nil，但不允许使用KVC给值类型设置nil。

​	nil是给OC对象用来初始化的，不能赋值给值类型，因此如果使用KVC操作的成员变量如果是值类型，设置nil时，系统崩溃。



​	当KVC给值类型设置nil时：

	> 1、查看该类是否实现setNilValueForKey:方法，实现则调用并结束KVC，否则进行第2步。
	>
	> 2、程序崩溃。



### 找不到key

当KVC找不到对应的key时：

> 1、查看该类是否实现了setValue:forUndefinedKey:或者valueForUndefinedKey:，实现了则调用该方法，并结束KVC，否则执行第2步。
>
> 2、程序崩溃。



使用keyPath时，和使用key一样，字符串中哪个节点触发了着两种异常，则查看该节点对应的类是否实现了相关方法。例如上面的"idCard._idCard"，如果查找不到"__idCard"，则查看ZPIDCard类是否实现相关方法。



## KVC的键值验证

​	该功能其实是，用来验证key对应的value是否符合要求，需要在使用KVC的类中实现相应的验证方法。

```objective-c
@interface ZPPerson5 : NSObject
@property (nonatomic, assign) NSInteger age;
@end

@implementation ZPPerson5
- (BOOL)validateValue:(inout id  _Nullable __autoreleasing *)ioValue forKeyPath:(NSString *)inKeyPath error:(out NSError * _Nullable __autoreleasing *)outError
{
    NSNumber *age = *ioValue;
    if (age.integerValue < 10) {
        return NO;
    }
    
    return YES;
}
@end

// main
ZPPerson5 *p = [ZPPerson5 new];
NSNumber *age = @(5);
NSError * __autoreleasing error = nil; // 指向指针的指针需要是否__autoreleasing修饰
BOOL s = [p validateValue:&age forKeyPath:@"age" error:&error];
NSLog(@"");
```

