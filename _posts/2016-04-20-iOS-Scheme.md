---
layout: post
title: "iOS—Scheme是什么、怎么自定义Scheme、JLRoutes的使用"
date: 2016-04-20
author: "Asingers"
catalog: true
subtitle: "URL Scheme整理"
categories: ios
tags:
   - iOS
---


## URL Scheme是什么

相信大家都知道URL。

[http://www.apple.com就是一个URL。](http://www.apple.com)

而://之前的部分就称为Scheme

也就是说 **http://www.apple.com的Scheme就是http。**  

## iOS中的URL Scheme

iOS中的Scheme也是一样的，无非是定义应用自己的Scheme，然后定义一些自己的URL解析，就好像YourApp://OneController?username=xxx&userInput=xxx

有了这些URL Scheme你可以像网页跳转一样通过URL来传递参数、信息。

比如常见的分享功能，从其他应用点击微信分享，会自动跳转到微信APP的朋友圈发表动态页面，并填好相应的动态内容。你可以想象一下其URL Scheme可能是这样的：weixin://dl/moments?content=”今天在学习URL Scheme”&src=”baidu.com”（我只是举个例子）

有一点需要注意的是，和Web开发不同，iOS中并不是所有的页面或者操作都有URL Schemes，这完全是由你主导的的，如果你需要，你就可以自己定义一些，并去解析。

## 什么时候用到URL Scheme

自定义Scheme是有意义的
有以下几种使用场景供你参考：

从一个页面跳转到另一个页面，你不想写N多行代码来『获取下一个控制器』->『创建控制器』->『传递参数  

从其他应用中跳转到你的应用中特定的位置，并填好相应的参数。比如微博分享的时候，是从另一个页面跳转到微博应用的『发微博』页面，并自动填好了微博的文字内容

### 使用浏览器访问应用

我们建一个应用，就叫URLSchemeDemo

在storyboard中，给我们的应用加一个按钮，便于展示
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-01.png-w500" alt="" class="shadow"/>

	
打开info.plist
- 添加一行，key选择 URL types
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-02.png-w500" alt="" class="shadow"/>

- 点击左边箭头打开列表，可以看到 Item 0。打开Item 0，可以看到 URL Identifier，这是你自定义的 URL scheme 的名字。如果想保证唯一性，可以使用翻转域名比如 com.taobao.ios.yourApp
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-03.png-w500" alt="" class="shadow"/>

- 给 Item 0 再新增一行，从下拉列表中选择 URL Schemes。你会发现这是一个Array，这是因为允许应用定义多个 URL schemes
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-04.png-w500" alt="" class="shadow"/>

- 打开URL schemes并点击里面的Item 0。在value中定义你的 URL scheme 的名字。比如你的APP名
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-05.png-w500" alt="" class="shadow"/>


 在AppDelegate.m中要处理接收到的URL Scheme

	-(BOOL)application:(UIApplication*)application openURL:	(NSURL*)url sourceApplication:(NSString*)sourceApplication 	annotation:(id)annotation {
	
	NSLog(@"从哪个app跳转而来 Bundle ID: %@", sourceApplication);
	NSLog(@"URL scheme:%@", [url scheme]);
    
	returnYES;
	}

1. 运行项目，当app安装到设备上时，URL Scheme将会自动注册
2. 打开Safari在地址栏输入URLSchemeDemo://（你刚刚在URL schemes中定义的Scheme）
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-06.png-w375" alt="" class="shadow"/>

3. 回车调整转，Safari会提示你『在URLSchemeDemo中打开连接吗？』
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-07.png-w375" alt="" class="shadow"/>

4. 点击确认，你会发现跳转到了你的应用中，并且后台也打印了相应的处理内容
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-08.png-w375" alt="" class="shadow"/>

<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-09.png-w500" alt="" class="shadow"/>



### 使用另一个应用访问应用

上面编写了如何从浏览器通过URL Scheme跳转应用，下面将展示如何从另一个应用跳转到本应用

再建一个项目，就叫URLSchemeDemoTest

在storyboard中拉一个按钮
<img src="http://7xnrog.com1.z0.glb.clouddn.com/blog_iOS%E2%80%94%E2%80%94Scheme%E6%98%AF%E4%BB%80%E4%B9%88-%E6%80%8E%E4%B9%88%E8%87%AA%E5%AE%9A%E4%B9%89Scheme-JLRoutes%E7%9A%84%E4%BD%BF%E7%94%A8-10.png-w375" alt="" class="shadow"/>

给按钮添加事件

	- (IBAction)jump:(UIButton *)sender {
		NSString *customURL = @"URLSchemeDemo://";
    	[[UIApplication sharedApplication] openURL:[NSURL URLWithString:customURL]];
    
			}

运行项目，点击按钮，你会发现同样能跳转到之前的应用


# JLRoutes

看到这里可能有人问了，我可以在跳转的时候传递一些参数吗？

当然可以，这些参数你都可以自己添加，但是同样要在`- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation`中做解析。
比如像YourAPP://SecondController?content=”成功解析”这样的URL Scheme，可能自己解析起来非常的费劲

在这里介绍一个第三方工具[JLRoutes](https://github.com/joeldev/JLRoutes)，可以非常方便的解析自定义URL Scheme

## 使用JLRoutes

比如我们现在就要解析URLSchemeDemo://SecondController，希望使用这个URLScheme直接可以打开URLSchemeDemo应用中的SecondController

### URLSchemeDemo项目

 导入JLRoutes.h、JLRoutes.m
我在URLSchemeDemo中添加SecondViewController
给SecondViewController在viewDidLoad中添加以下颜色，以作区分

	- (void)viewDidLoad {;
    	[superviewDidLoad];
    	self.view.backgroundColor= [UIColorgreenColor];
    	};

在AppDelegate.m中修改处理方式

	#import"AppDelegate.h"
	#import"JLRoutes.h"
	
	@interfaceAppDelegate()
	
	@end
    
	@implementationAppDelegate
    
	- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {
    	[JLRoutes addRoute:@"/:controller"handler:^BOOL(NSDictionary*parameters) {
    	NSString*controller = parameters[@"controller"];
    
	[self.window.rootViewControllerpresentViewController:[[NSClassFromString(controller) alloc] init] animated:YEScompletion:^{
    
	}];
	return YES;
	}];
	return YES;
    	}
    
	- (BOOL)application:(UIApplication*)application openURL:(NSURL*)url sourceApplication:(NSString*)sourceApplication annotation:(id)annotation {
		return[JLRoutes routeURL:url];
	}
    
	@end
    
### URLSchemeDemoTest项目

当然是改一下我们点击按钮后打开的URL

    - (IBAction)jump:(UIButton *)sender {;
    NSString *customURL = @"URLSchemeDemo://SecondViewController";
    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:customURL]];
    
    };
    

### 运行

1. 打开URLSchemeDemoTest应用，点击按钮，就可以直接跳转到URLSchemeDemo的SecondViewController了

**[Demo](http://download.csdn.net/detail/u010127917/9387848)**

### 补充

JLRoutes是一个非常好用的工具，除了以上简单的用法外，还可以解析更加复杂的URL Scheme，可以参考官方文档：[https://github.com/joeldev/JLRoutes](https://github.com/joeldev/JLRoutes)