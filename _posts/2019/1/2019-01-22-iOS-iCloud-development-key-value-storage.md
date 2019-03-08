---
layout: post
title: iOS iCloud Development(1) Key-Value Storage
subtitle: iCloud 存储之 Key-Value
categories: ios
header-style: text
tags: 
    - iOS

---
   

### 配置iCloud 需要付费开发者权限

	TARGETS -> Capabilities -> iCloud
	
同样需要在开发者中心创建相应App ID，并在App Service 选项中勾选iCloud

#### Key-value storage ####

在Xcode中 iCloud下边一共有三个可以勾选的服务，其中第一个就是key-value storage，这个也是最简单的iCloud使用方法了，他跟NSUserDefaults的使用方法基本一样，都是以键值对的方式存储数据。只不过处理iCloud的类为 `NSUbiquitousKeyValueStore`。并且存储在云端。

#### 存储数据

存储数据的方式很简单，只要使用 setObject:forkey:之后，使用synchronize同步就可以了。

	NSUbiquitousKeyValueStore *keyValueStore = [NSUbiquitousKeyValueStore defaultStore];
	[keyValueStore setObject:@"test message" forKey:@"YJTEST"];
	[keyValueStore synchronize];
	
#### 获取数据

获取数据的方式也一样，直接食用`objectForKey `取值。

	NSUbiquitousKeyValueStore *keyValueStore = [NSUbiquitousKeyValueStore defaultStore];
	NSString *testString = [keyValueStore objectForKey:@"YJTEST"];
	NSLog(@"%@",testString);
	
#### 数据改变通知

	
	 [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(storeDidChange:) name:NSUbiquitousKeyValueStoreDidChangeExternallyNotification object:[NSUbiquitousKeyValueStore defaultStore]];
	 
这通知东西有什么用呢？你回想一下一些App删除之后重新安装，为什么还能记住你之前的登录账户信息。✌️

所以一些个人开发者，想要存之一些简单的键值信息，iCloud Key-value 存储是最为方便和高效的方法。