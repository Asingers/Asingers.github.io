---
layout: post
title: "OC 中使用 Swift"
date: 2016-09-27
author: "Alpaca"
subtitle: "使用Swift控件"
catalog: true
categories: ios
tags:
   - iOS
   - Swift
   
---

以使用Swift写的一个控件为例:  

#### 桥接文件

通常我们需要在OC项目中创建一个Swift类 此时Xcode提示创建一个桥接文件  
![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-09-27-1.png)  
![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-09-27-2.png)  

#### Defines Module

设置为Yes
![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-09-27-3.png)  

#### 引用头文件

导入外部Swift文件 并引用"项目名-swift.h"  编译没错说明头文件已经生成了 虽然导航栏看不到
![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-09-27-5.png)  

#### 调用 

语法同OC,调用Swift中的方法 这样我们就可以在OC中使用Swift写的控件了
 ![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-09-27-6.png)  
 
 在实现Swift中代理方法的时候发现找不到这个代理方法,需要加 @objc  
 ![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-09-27-%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-09-27%20%E4%B8%8B%E5%8D%883.27.41.png)  

再次编译之后就可以实现了 
![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-09-27-4.png)  


