# KVO

​	KVO(键值观察)，是iOS实现的无侵入式的观察者模式，该功能是基于runtime和KVC来完成的。

## 功能

### 自动监听

#### 实现自动监听

> 1、在观察者类中实现
>
> ```objectivec
> - (void) observeValueForKeyPath:(NSString *)keyPath
>                        ofObject:(id)object 
>                          change:(NSDictionary *)change
>                         context:(void *)context
> ```
>
> 2、使用下面三个方法，添加和删除观察者
>
> ```objective-c
> // 添加观察者
> - (void)addObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath options:(NSKeyValueObservingOptions)options context:(nullable void *)context;
> 
> // 删除观察者
> - (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath context:(nullable void *)context
> 
> // 删除观察者
> - (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath;
> ```

##### 监听方法中参数说明

> keyPath：监听的属性名称，可以是属性的属性。
>
> object：被观察的对象
>
> change：根据注册时设置的枚举，监听时设置不同值，常用情况是记录该属性变化前后的值。
>
> context：上下文环境，用来区分当前观察者，因为KVO允许多对多监听。
>
> options: 说明监听类型，监听更新后的值，监听更新之前的值，监听动作等。

>NSKeyValueObservingOptionNew：监听更新后的值，NSKeyValueChangeNewKey读取
>
>NSKeyValueObservingOptionOld：监听更新前的值，NSKeyValueChangeOldKey读取。
>
>NSKeyValueObservingOptionInitial，注册KVO时，触发监听调用，修改值时也会触发监听。
>
>NSKeyValueObservingOptionPrior：每次修改前触发一次监听，因此与其他枚举搭配会触发两次监听，

#### KVO可以监听的数据类型 

	> 1、类中定义的属性。
	>
	> 2、类中定义的私有成员变量，无getter和setter方法。
	>
	> 3、类中只有一个符合OC要求的setter 方法，无相应属性和成员变量。
	>
	> 4、类中属性的属性，其实是会监听keypath上的每个节点。(在原理章节再说明))
	>
	> 5、类中集合属性，给其赋值可以触发KVO，但是如果增删改集合的元素不会触发。
	>
	> 
	>
	> 总结：由上面可以看出KVO是基于KVC的(在原理章节再说明)，因此想要实现实现KVO监听，必须要至少具备以下条件的一种：
	>
	> 1、类中有对应的成员变量。
	>
	> 2、类中有相应的setter方法，例如对分类定义的属性的监听。



##### 对于集合的KVO监听

​	通常KVO只能监听变量本身内容的改变，对于像集合变量的增删改的变化，KVO无法监听。因此可以借助KVC的帮助进行监听。

```objective-c
// forKey
- (NSMutableArray *)mutableArrayValueForKey:(NSString *)key;
- (NSMutableOrderedSet *)mutableOrderedSetValueForKey:(NSString *)key;
- (NSMutableSet *)mutableSetValueForKey:(NSString *)key;

// forKeypath
- (NSMutableArray *)mutableArrayValueForKeyPath:(NSString *)keyPath;
- (NSMutableOrderedSet *)mutableOrderedSetValueForKeyPath:(NSString *)keyPath;
- (NSMutableSet *)mutableSetValueForKeyPath:(NSString *)keyPath;
```



实例，使用KVC获取集合就可以触发KVO监听了。

```objective-c
@interface ZPPerson7 : NSObject
@property (nonatomic, copy) NSString *nameAutoKVO;
@property (nonatomic, strong)NSMutableArray *arrM;
@end

// main   
// 监听集合
ZPPerson7 *p8 = [ZPPerson7 new];
p8.arrM = [NSMutableArray arrayWithCapacity:3];
ZPObserverOfPerson6 *obs5 = [ZPObserverOfPerson6 new];
[p8 addObserver:obs5 forKeyPath:@"arrM" options:NSKeyValueObservingOptionOld | NSKeyValueObservingOptionNew context:nil];
// 使用KVC获取集合属性
NSMutableArray *arr = [p8 mutableArrayValueForKey:@"arrM"];
[arr addObject:@"1111"];

// 直接这样增删改数组元素不会触发KVO监听
[p8.arrM addObject:@"2222"];
[p8 removeObserver:obs5 forKeyPath:@"arrM"];
```



#### KVO键值观察依赖

​	类中属性有时是依赖其他属性的，例如姓名全称与属性姓和名是存在依赖关系的，当姓或名改变时，姓名全称也需要改变。

实现观察依赖的步骤：

> 1、在建立依赖的类中实现+keyPathsForValuesAffectingValueForKey:方法，如果是属性的话，则实现+keyPathsForValuesAffecting<Key>方法，用来返回一个依赖关联集合。
>
> 2、实现自动监听步骤。

观察依赖支持的数据类型	

>只要支持KVC的读写就行，即可以是无setter、getter方法的私有成员变量，可以是只有getter和setter方法没有私有成员变量。

```objective-c
@interface ZPPerson6 : NSObject
@property (nonatomic, assign) NSInteger age;
@property (nonatomic, strong) ZPPerson6 *father;
@property (nonatomic, strong) ZPPerson6 *mother;
@property (nonatomic, assign) NSUInteger totalAge1; // 家庭年龄总和
@end

@interface ZPPerson6 ()
{
    NSUInteger _totalAge2; // 家庭年龄总和
}
@end

@implementation ZPPerson6
  // 家庭成员每修改一次年龄，都需要修改_totalAge2。由于没有getter方法，需要自己统计。
- (void)setAge:(NSInteger)age{
    NSInteger totalAge = [[self.son valueForKey:@"totalAge2"] integerValue];
    [self.son setTotalAge4Value:(totalAge - _age + age)];
    _age = age;
}

// 重写totalAge1的getter方法，观察者的observeValueForKeyPath...方法是通过KVC来获取观察对象的值。
- (NSUInteger)totalAge1{
    return self.father.age + self.mother.age + self.age;
}

// 建立依赖方式1，totalAge1依赖于父母和子女的年龄。
+ (NSSet<NSString *> *)keyPathsForValuesAffectingTotalAge1
{
    return [NSSet setWithObjects:@"father.age", @"mother.age", @"age", nil];
}

// 建立依赖方式2，totalAge2依赖于父母和子女的年龄。
+ (NSSet<NSString *> *)keyPathsForValuesAffectingValueForKey:(NSString *)key{
    NSArray *moreKeyPaths = nil;
    if ([key isEqualToString:@"totalAge2"]) {
        moreKeyPaths = [NSArray arrayWithObjects:@"father.age", @"mother.age", @"age", nil];
    }

    NSSet *keyPaths = [super keyPathsForValuesAffectingValueForKey:key];
    if (moreKeyPaths) {
        keyPaths = [keyPaths setByAddingObjectsFromArray:moreKeyPaths];
    }

    return keyPaths;
}

-(void)setTotalAge2Value:(NSInteger)value{
    _totalAge1 = value;
}

@end
```



​	上面说明了两种建立依赖的方式，方式1适合属性(使用@property定义的成员变量)，方式2适合非属性(私有成员变量，或者没有成员变量只有setter和getter方法。)

​	方式2写法，调用父类方法获取该key的依赖集合，将建立的新依赖保存到这个依赖集合并返回。



### 手动监听

​	手动监听步骤：

> 1、在需要监听的类中实现+automaticallyNotifiesObserversForKey:方法，并关闭该属性的自动KVO监听，返回NO就是告诉系统，建立该key的KVO时，不要建立自动KVO。
>
> 2、重写该key对应的setter方法，并在方法中调用willChangeValueForKey:和didChangeValueForKey方法。
>
> 
>
> 这样在添加该属性监听时，就是手动监听。



​	+automaticallyNotifiesObserversForKey:

> 该方法用来控制该类中哪些key需要手动监听，当返回NO时，则需要手动建立监听；当返回YES时，由系统建立监听。

​	willChangeValueForKey:和didChangeValueForKey:

	> 这两个方法用来通知观察者，观察对象发生改变，一般用在观察key的相应setter方法中，在赋值语句前后添加这两句代码。



## KVO异常

>1、添加和删除KVO时，keyPath设置为nil，会导致程序崩溃。
>
>2、添加了观察者，但未实现 `observeValueForKeyPath:ofObject:change:context:` 方法，导致崩溃。
>
>3、添加和删除KVO语句不匹配，导致崩溃。
>
>​	1). 删除未注册的KVO，会导致程序崩溃。
>
>​	2). 添加KVO，但是当被观察者被释放但未删除KVO，导致崩溃，iOS10之后不会崩溃。
>
>​	3). 添加KVO，但是观察者被释放但未删除KVO，修改观察属性时，导致野指针。
>
>​	
>
>注意：避免使用KVO导致的系统崩溃问题，添加次数和删除次数一定要匹配，即使对监听者对同一个观察者的key添加多次监听，只要最后调用相同次数的移除操作，就不会有问题，也就是上面说的添加和删除语句要匹配。



## KVO原理

​	KVO是利用了对象的isa指针，实现了对监听对象无代码侵入式的监听。

​	给监听对象添加KVO时，系统所做的步骤：

	>1、创建一个该监听对象的类的派生类。
	>
	>2、重写相应key的setter方法，class方法、dealloc方法，并添加isKVO方法。
	>
	>3、修改对象的isa指针，指向派生类。

​	删除监听对象的KVO时，系统所做的步骤：

	>在dealloc中修改监听对象的isa指针，指向原来的类，销毁生成的派生类。

```objective-c
ZPPerson *p = [ZPPerson new];
ZPObserverOfPerson6 *obs1 = [ZPObserverOfPerson6 new];
    [p1 addObserver:obs1 forKeyPath:@"nameAutoKVO" options:NSKeyValueObservingOptionOld|NSKeyValueObservingOptionNew context:nil];

// 系统生成ZPPerson类的派生类NSKVONotifying_ZPPerson，重写setter方法
-(void)setNameAutoKVO(NSString *)nameAutoKVO{
  [self willChangeValueForKey:@"nameAutoKVO"];
  [self setValue:nameAUtoKVO forKeyPath:@"nameAutoKVO"];
  [self didChangeValueForKey:@"nameAutoKVO"];
}
```



### KVO 触发条件

​	在注册KVO后的三种触发KVO监听的条件

> 1、显示调用willChangeValueForKey:和didChangeValueForKey:方法。
>
> 2、使用KVC设置对应key的值。
>
> 3、使用点语法，设置对应key的值。



> 上面三种方法原理都是一样的，都是通过willChangeValueForKey:和didChangeValueForKey:方法触发KVO，后两者是通过调用派生类重写的setter方法，直接访问对象成员变量是无法触发KVO监听的。

### 属性的属性的KVO

​	给属性的属性添加KVO监听时，Runtime会给属性所在的每个类生成一个派生类，并修改isa指针，因此像下面ZPPerson6和ZPPerson7的类都会生成对应的派生类，并修改他们的isa指针指向这些派生类。`因此修改keyPath上任意节点的值，都会触发KVO监听，像下面代码修改“son.nameAutoKVO”的son值也会触发KVO监听。`

```objective-c
@interface ZPPerson6 : NSObject
@property (nonatomic, copy) NSString *nameAutoKVO;
@end

@interface ZPPerson7 : NSObject

@end

@interface ZPPerson7 ()
{
    ZPPerson6 *_son;
}
@end
@implementation ZPPerson7

@end

// main
void main(){
   ZPObserverOfPerson6 *obs4 = [ZPObserverOfPerson6 new];
    ZPPerson7 *p7 = [ZPPerson7 new];
	  ZPPerson6 *p6 = [ZPPerson6 new];
    [p7 setValue:p6 forKey:@"son"];
  
    [p7 addObserver:obs4 forKeyPath:@"son.nameAutoKVO" options:NSKeyValueObservingOptionOld | NSKeyValueObservingOptionNew context:nil];
  	// 修改son对象的nameAutoKVO，触发KVO。
    ZPPerson6 *tempSon = [p7 valueForKey:@"son"];
    tempSon.nameAutoKVO = @"json";
  	//修改p7的son成员变量，也会触发KVO
    [p7 setValue:p1 forKey:@"son"];
}
```



#### KVO生成派生类的伪代码

```objective-c
// NSKVONotifying_ZPPerson7
@interface NSKVONotifying_ZPPerson7 : NSObject

@end

@interface NSKVONotifying_ZPPerson7 ()
{
    ZPPerson6 *_son;
}
@end
@implementation NSKVONotifying_ZPPerson7

- (void)setSon:(ZPPerson6 *)son{
  [self willChangeValueForKey:@"son.nameAutoKVO"];
  [super setValue:son forKey:@"son"];
  [self didChangeValueForKey:@"son.nameAutoKVO"];
}

-(ZPPerson6 *)son{
  return [super valueForKey:@"son"];
}
@end

// NSKVONotifying_ZPPerson6
@interface NSKVONotifying_ZPPerson6 : NSObject
@property (nonatomic, copy) NSString *nameAutoKVO;
@end
  
@implementation NSKVONotifying_ZPPerson6

- (void)setNameAutoKVO:(NSString *)nameAutoKVO{
  [self willChangeValueForKey:@"son.nameAutoKVO"];
  [super setValue:son forKey:@"nameAutoKVO"];
  [self didChangeValueForKey:@"son.nameAutoKVO"];
}

-(NSString *)nameAutoKVO{
  return [super valueForKey:@"nameAutoKVO"];
}
@end
```

无论修改son，还是son.nameAutoKVO都会触发KVO监听。

### KVO流程

​	我们拿上面例子，给"son.nameAutoKVO"赋值的步骤：

	>  1、调用NSKVONotifying_ZPPerson7类的son成员的getter方法，并在方法中使用KVC获取原类中的son值。
	>
	>  2、调用son对象指向的派生类NSKVONotifying_ZPPerson6的setNameAutoKVO方法。
	>
	>  3、在该setter方法中通知观察者，并使用KVC给原类的nameAutoKVO成员变量赋值。

​	`从这里也可以看出触发KVO的流程，进入派生类的setter方法，调用willChangeValueForKey:和didChangeValueForKey:，并且使用KVC设置父类(原类)的属性`。



## KVO与线程

​	KVO并不是线程安全的，具有同步特性(线性执行)，不建议在多线程中使用KVO，除非你能保证KVO在多线程中的线程安全问题。

