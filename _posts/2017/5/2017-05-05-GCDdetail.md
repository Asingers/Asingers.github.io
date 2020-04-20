---
layout: post
title: 理解线程
subtitle: GCD
catalog: true
header-img: https://ws2.sinaimg.cn/large/006tNc79ly1fiskbwpfupj30xc0m8td6.jpg
categories: ios
header-mask: 0.4
tags: 
    - iOS
    - 线程
    - GCD
   
---

#### 含义

首先我们来理解 GCD 的含义:

- GCD全称是Grand Central Dispatch

- GCD是苹果公司为多核的并行运算提出的解决方案

- GCD会自动利用更多的CPU内核（比如双核、四核）

- GCD会自动管理线程的生命周期（创建线程、调度任务、销毁线程）

再来理解 串并行,同异步:

#### 串行队列同步执行: 在当前线程顺序执行 

	dispatch_queue_t queue =  dispatch_queue_create("queneName", NULL);
     //2.添加任务到队列中，就可以执行任务
    dispatch_sync(queue, ^{
        NSLog(@"下载图片1----%@",[NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"下载图片2----%@",[NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"下载图片3----%@",[NSThread currentThread]);
    });
        //打印主线程
    NSLog(@"主线程----%@",[NSThread mainThread]);
    
    下载图片1----<NSThread: 0x600002b20240>{number = 1, name = main}
    下载图片2----<NSThread: 0x600002b20240>{number = 1, name = main}
    下载图片3----<NSThread: 0x600002b20240>{number = 1, name = main}
    主线程----<NSThread: 0x600002b20240>{number = 1, name = main}
    
*不会开启新的线程，创建的自定义队列无效。*

#### 串行队列异步执行: 开辟一条新的线程,在该线程中顺序执行
	
	    dispatch_queue_t queue =  dispatch_queue_create("queneName", NULL);
        //2.添加任务到队列中，就可以执行任务
    dispatch_async(queue, ^{
        NSLog(@"下载图片1----%@",[NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"下载图片2----%@",[NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"下载图片3----%@",[NSThread currentThread]);
    });
        //打印主线程
    NSLog(@"主线程----%@",[NSThread mainThread]);
    
    下载图片1----<NSThread: 0x600001308040>{number = 5, name = (null)}
    下载图片2----<NSThread: 0x600001308040>{number = 5, name = (null)}
    下载图片3----<NSThread: 0x600001308040>{number = 5, name = (null)}
    主线程----<NSThread: 0x600001344840>{number = 1, name = main}

*异步串行执行3个任务，只会开启一个子线程。*
	
#### 并行队列同步执行: 不开新线程,在当前线程顺序执行
	
	        //获取全局并发队列
    dispatch_queue_t queue =  dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
        //2.添加任务到队列中，就可以执行任务
    dispatch_sync(queue, ^{
        NSLog(@"下载图片1----%@",[NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"下载图片2----%@",[NSThread currentThread]);
    });
    dispatch_sync(queue, ^{
        NSLog(@"下载图片3----%@",[NSThread currentThread]);
    });
        //打印主线程
    NSLog(@"主线程----%@",[NSThread mainThread]);
    
    下载图片1----<NSThread: 0x600001420080>{number = 1, name = main}
    下载图片2----<NSThread: 0x600001420080>{number = 1, name = main}
    下载图片3----<NSThread: 0x600001420080>{number = 1, name = main}
    主线程----<NSThread: 0x600001420080>{number = 1, name = main}
	
*不会开启新的线程，并发队列失去了并发的功能。*

#### 并行队列异步执行:开辟多个线程,无序执行
	
	   //获取全局并发队列
    dispatch_queue_t queue =  dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    //2.添加任务到队列中，就可以执行任务
    dispatch_async(queue, ^{
       NSLog(@"下载图片1----%@",[NSThread currentThread]);
       });
    dispatch_async(queue, ^{
       NSLog(@"下载图片2----%@",[NSThread currentThread]);
           });
    dispatch_async(queue, ^{
       NSLog(@"下载图片3----%@",[NSThread currentThread]);
        });
    //打印主线程
    NSLog(@"主线程----%@",[NSThread mainThread]);
    
    下载图片3----<NSThread: 0x600000af9680>{number = 5, name = (null)}
    下载图片2----<NSThread: 0x600000afdac0>{number = 7, name = (null)}
    下载图片1----<NSThread: 0x600000ae0200>{number = 6, name = (null)}
    主线程----<NSThread: 0x600000ab8d40>{number = 1, name = main}
	
*异步并发执行3个任务，会开启3个子线程。*

#### 主队列异步执行: 不开新线程,同步执行
	
	dispatch_async(dispatch_get_main_queue(), ^{
        NSLog(@"111111");
    });
    NSLog(@"222222");
    
    2019-01-16 10:28:57.254042+0800 GCD[463:148602] 222222
    2019-01-16 10:28:57.277710+0800 GCD[463:148602] 111111
	
#### 主队列同步执行: 会造成死锁. 
	⚠️ BOOM！
	
	dispatch_sync(dispatch_get_main_queue(), ^{
       NSLog(@"111111");
    });
     
	NSLog(@"222222")


#### 总结
总结一下就是: 

- 同异步决定了要不要去开辟一个新的线程.
- 串并行决定了任务的执行方式.

当然除了上边的方法,创建一个`串行队列`也可以用:
	
1. 使用dispatch_queue_create函数创建串行队列
	
       dispatch_queue_t queue = dispatch_queue_create("queneName", NULL); // 创建
	
2. 还有就是主队列是 GCD 自带的一个特殊的串行队列  

       dispatch_queue_t queue = dispatch_get_main_queue();
	
创建一个`串行队列`也可以用
`dispatch_get_global_queue`函数获得全局的并发队列  
	
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0); // 获得全局并发队列
其中,全局并发队列有优先级:
 
	#define DISPATCH_QUEUE_PRIORITY_HIGH 2 // 高
 
	#define DISPATCH_QUEUE_PRIORITY_DEFAULT 0 // 默认（中）
 
	#define DISPATCH_QUEUE_PRIORITY_LOW (-2) // 低
 
	#define DISPATCH_QUEUE_PRIORITY_BACKGROUND INT16_MIN // 后台

尝试用图表总结一下下:
![](http://o6ledomfy.bkt.clouddn.com/20170822150339939537875.jpg)



