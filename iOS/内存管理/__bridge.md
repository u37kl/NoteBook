# Bridge

​	bridge又名桥接，用来做Foundation和Core Foundation之间的数据类型转换用的。

​	苹果提供了三种方式：

	> __bridge
	>
	> __bridge_retained
	>
	> __bridge_transfer

## __bridge

​	将OC对象转成Core Foundation框架中的数据类型，但是不转交对象的内存管理，换句话说还是由ARC来管理对象，当对象被销毁时，C变量会变成野指针。

```objective-c
void * p;
{
	ZPPerson *zp1 = [ZPPerson new];
	p = (__bridge void *)zp1;
	NSLog(@"%ld", CFGetRetainCount(p)); // 1
}
NSLog(@""); // 野指针警告
```



## __bridge_retained

​	将OC对象转成Core Foundation框架中的数据类型并且转交对象的内存管理，换句话说转换后，对象的引用计数器+1，需要开发者在使用完后手动调用CFRelease函数释放。

```objective-c
void * p1 = NULL;
{
      ARCTest1 *t1 = [ARCTest1 new];
	    p1 = (__bridge_retained void *)t1;
      NSLog(@"%ld", CFGetRetainCount(p1));   //2
}
NSLog(@"%ld", CFGetRetainCount(p1)); // 1
CFRelease(p1);
```

> 使用“__bridge_retained”转换后，C指针获得了对象的持有权，它的释放需要开发者自己管理，即OC指针的持有权由ARC管理，C指针的持有权由开发者管理。



## __bridge_transfer

​	将Core Foundation的对象转换成OC对象，并移交内存管理权限，有ARC管理对象的引用计数器。`但是要注意，转换后的C指针将变成野指针，需要置为NULL`。

```objective-c
void *p2 = NULL;
{
   ARCTest1 *t1 = [ARCTest1 new];
	 p2 = (__bridge_retained void *)t1;
   NSLog(@"%ld", CFGetRetainCount(p2));   // 2
}
NSLog(@"%ld", CFGetRetainCount(p2)); // 1
{
   ARCTest1 *t1 = (__bridge_transfer ARCTest1 *)p2;
   NSLog(@"%ld", CFGetRetainCount(p2));  // 1
   NSLog(@"");	
}

p2 = NULL; // 对象释放后它不会自动置为null，因此一定不要忘了将p2置为null，否则就是野指针。
```



```objective-c
// 上面代码转换成MRC
void *p2 = NULL;
{
   ARCTest1 *t1 = [ARCTest1 new];
   [t1 retain];
	 p2 = (__bridge_retained void *)t1;
   CFRetain(p2);
   NSLog(@"%ld", CFGetRetainCount(p2));   // 2
}
NSLog(@"%ld", CFGetRetainCount(p2)); // 1
{
   ARCTest1 *t1 = (__bridge_transfer ARCTest1 *)p2;
   [t1 retain];
   CFRelease(p2);
  
   NSLog(@"%ld", CFGetRetainCount(p2));  // 1
   NSLog(@"");
   [t1 release]; 
}

p2 = NULL; // 对象释放后它不会自动置为null，因此一定不要忘了将p2置为null，否则就是野指针。
```

> 因此使用"__bridge_transfer"转换时，释放C指针的对象持有权，添加OC指针的对象持有权。

## 苹果对C中的内存管理函数

> CFRetain()：使对象的引用计时器+1
>
> CFRelease()：使对象的引用计时器-1
>
> CFGetRetainCount()：获取对象的引用计时器