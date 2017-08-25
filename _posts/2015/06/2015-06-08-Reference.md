---
layout: post
title: 引用计数与 ARC
subtitle: 你说的白是什么白
header-img: http://o6ledomfy.bkt.clouddn.com/20170825150362958121722.jpg
categories: ios
catalog: true
header-mask: 0.3
tags: 
    - iOS

---

iOS5以前自动引用计数（ARC）是在MacOS X 10.7与iOS 5中引入一项新技术，用于代替之前的手工引用计数MRC（Manual Reference Counting）管理Objective-C中的对象【官方也叫MRR（Manual Retain Release）】。如今，ARC下的iOS项目几乎把所有内存管理事宜都交给编译器来决定，而开发者只需专注于业务逻辑。

#### 引用计数
引用计数（Reference Count），也叫保留计数（retain count），表示对象被引用的次数。一个简单而有效的管理对象生命周期的方式。

#### 与内存管理的关系

在Objective-C内存管理中，每个对象都有属于自己的计数器：如果想让某个对象继续存活(例如想对该对象进行引用)，就递增它的引用计数；当用完它之后，就递减该计数；当没人引用该对象，它的计数变为0之后，系统就把它销毁。 

这个，就是引用计数在其中充当的角色：用于表示当前有多少个对象想令此对象继续存活程序中；

#### 工作原理

1. 当我们创建(alloc)一个新对象A的时候，它的引用计数从零变为 1；
2. 当有一个指针指向这个对象A，也就是某对象想通过引用保留(retain)该对象A时，引用计数加 1；
3. 当某个指针/对象不再指向这个对象A，也就是释放(release)该引用后，我们将其引用计数减 1；
4. 当对象A的引用计数变为 0 时，说明这个对象不再被任何指针指向(引用)了，这个时候我们就可以将对象A销毁，所占内存将被回收，且所有指向该对象的引用也都变得无效了。系统也会将其占用的内存标记为“可重用”(reuse)；

![](http://o6ledomfy.bkt.clouddn.com/20170825150362787484515.jpg)

#### 操作引用计数的方法

- retain ： 保留。保留计数+1；
- release ： 释放。保留计数 -1；
- autorelease ：稍后(清理“自动释放池”时)，再递减保留计数，所以作用是延迟对象的release；
- dealloc方法：另外，当计数为0的时候对象会自动调用dealloc。而我们可以在dealloc方法做的，就是释放指向其他对象的引用，以及取消已经订阅的KVO、通知；（自己不能调用dealloc方法，因为运行期系统会在恰当的时候调用它，而且一旦调用dealloc方法，对象不再有效，即使后续方法再次调用retain。）

所以，调用release后会有2种情况：

调用前计数>1，计数减1；

调用前计数<1，对象内存被回收； 

- retainCount：获取引用计数的方法。
	
		[object retainCount]; 
		
retain作用：

调用后计数+1，保留对象操作。但是当对象被销毁、内存被回收的时候，即使使用retain也不再有效；

**autorelease作用：**

autorelease不立即释放，而是注册到autoreleasepool(自动释放池)中，等到pool结束时释放池再自动调用release进行释放工作。

autorelease看上去很像ARC，但是实际上更类似C语言中的自动变量（局部变量），当某自动变量超出其作用域(例如大括号)，该自动变量将被自动废弃，而autorelease中对象实例的release方法会被调用；[与C不同的是，开发者可以设定变量的作用域。]

**释放时间：** 每个Runloop中都创建一个Autorelease pool（自动释放池），每一次的Autorelease，系统都会把该Object放入了当前的Autorelease pool中，并在Runloop的末尾进行释放，而当该pool被释放时，该pool中的所有Object会被调用Release。 所以，一般情况下，每个接受autorelease消息的对象，都会在下个Runloop开始前被释放。

例如可用以下场景：(需要从ARC改为使用手动管理的可以做如下的设置： 在Targets的Build Phases选项下Compile Sources下选择要不使用ARC编译的文件，双击它，输入-fno-objc-arc即可使用MRC手工管理内存方式；)
	
	-(NSString *)getSting
	{
    	NSString *str = [[NSString alloc]initWithFormat:@"I am Str"];
 	   return [str autorelease];
	}
	
自动释放池中的释放操作会等到下一次时间循环时才会执行，所以调用以下： 
	
	NSString *str = [self getSting];
	NSLog(@"%@",str);
	
返回的str对象得以保留，延迟释放。因此可以无需再NSLog语句之前执行保留操作，就可以将返回的str对象输出。

所以可见autorelease的作用是能延长对象的生命期。使其在跨越方法调用边界后依然可以存活一段时间。

**release会立即执行释放操作，使得计减1；**

有这样一种情况：当某对象object的引用计数为1的时候，调用“[object release];”，此时如果再调用NSLog方法输出object的话，可能程序就会崩溃，当然只是有可能，因为对象所占内存在“解除分配(deallocated)”之后，只是放回“可用内存池（avaiable pool）”，但是如果执行NSLog时，尚未覆写对象内存，那么该对象依然有效，所以程序有可能不会崩溃，由此可见，因过早地释放对象而导致的bug很难调试。

为避免这种情况，一般调用完对象之后都会清空指针："object = nil"，这样就能保证不会出现指向无效对象的指针，也就是悬挂指针（dangling pointer）;

悬挂指针：指向无效对象的指针。

**那么，向已经释放(dealloc)的对象发送消息，retainCount会是多少？**
原则是不可以这么做。因为该对象的内存已经被回收，而我们向一个已经被回收的对象发了一个 retainCount 消息，所以它的输出结果应该是不确定的，例如为减少一次内存的写操作，不将这个值从 1 变成 0，所以很大可能输出1。例如下面这种情况：
	
	Person *person = [[Person alloc] init]; //此时，计数 = 1   
	[person retain];    //计数 = 2   
	[person release];   //计数 = 1   
	[person release];   //很可能计数 = 1;  
	
虽然第四行代码把计数1release了一次，原理上person对象的计数会变成0，但是实际上为了优化对象的释放行为，提高系统的工作效率，在retainCount为1时release系统会直接把对象回收，而不再为它的计数递减为0，所以一个对象的retainCount值有可能永远不为0；

因此，不管是否为ARC的开发环境中，也不推荐使用retainCount来做为一个对象是否存在于内存之中的依据。

#### ARC

**1.背景：**

ARC是iOS 5推出的新功能，全称叫 ARC(Automatic Reference Counting)。

即使2014 年的 WWDC 大会上推出的Swift 语言，该语言仍然使用 ARC 技术作为其管理方式。

**2.ARC是什么？**

需要注意的是，ARC并不是GC（Garbage Collection 垃圾回收器），它只是一种代码静态分析（Static Analyzer）工具，背后的原理是依赖编译器的静态分析能力，通过在编译时找出合理的插入引用计数管理代码，从而提高iOS开发人员的开发效率。 

Apple的文档里是这么定义ARC的:

“自动引用计数(ARC)是一个编译器级的功能，它能简化Cocoa应用中对象生命周期管理（内存管理）的流程。”

**3.ARC在做什么？**

在编译阶段，编译器将在项目代码中自动为分配对象插入retain、release和autorelease，且插入的代码不可见。

但是，需要注意的是，ARC模式下引用计数规则还起作用，只是编译器会为开发者分担大部分的内存管理工作，除了插入上述代码，还有一部分优化以及分析内存的管理工作。

**作用：**

a.降低内存泄露等风险 ； 
b.减少代码工作量，使开发者只需专注于业务逻辑；
4.ARC具体为引用计数做了哪些工作？

编译阶段自动添加代码：

编译器会在编译阶段以恰当的时间与地方给我们填上原本需要手写的retain、release、autorelease等内存管理代码，所以ARC并非运行时的特性，也不是如java中的GC运行时的垃圾回收系统；因此，我们也可以知道，ARC其实是处于编译器的特性。
	
	-(void)setup
	{
    	_person = [person new];
	}
	
在手工管理内存的环境下，_person是不会自动保留其值，而在ARC下编译，其代码会变成：

	-(void)setup
	{
    	person *tmp = [person new];
    	_person = [tmp retain];
    	[tmp release];
	}
	
当然，在开发工作中，retain和release对于开发人员来说都可以省去，由ARC系统自动补全，达到同样的效果。

但实际上，ARC系统在自动调用这些方法时，并不通过普通的Objective-C消息派发控制，而是直接调用底层C语言的方法：

比如retain，ARC在分析到某处需要调用保留操作的地方，调用了与retain等价的底层函数 objc_retain，所以这也是ARC下不能覆写retain、release或者autorelease的原因，因为这些方法在ARC从来不会被直接调用。
