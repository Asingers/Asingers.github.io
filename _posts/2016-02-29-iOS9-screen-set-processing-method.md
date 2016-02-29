---
layout: post
title: "iOS9横竖屏设置的处理方法"
subtitle: "iOS 9 somehow screen set processing method"
date: 2016-02-29 
author: "Asingers"
categories: ios
tags:
    - iOS
    - Dev
---
在一般的视频类APP播放的时候都会支持横屏，这样做的好处就是便于观看。你的项目中支持横屏吗？我们一起了解一下，在iOS9中横竖屏设置的处理方法吧！

## 支持横竖屏配置

在iOS6以后，如果APP需要支持横屏，需要在xcode设置中General里面进行勾选配置：

<img src="http://images.90159.com/12/orientation1.png" alt="" class="shadow"/>


配置完成之后，我们可以看一下Info.plist里面的Supported interface orientations选项也相应的改变了。如下图：

<img src="http://images.90159.com/12/orientation2.png" alt="" class="shadow"/>


当然，我们也可以直接在Info.plist进行配置。

## 支持横竖屏方法

在iOS6之前我们可以直接用这个方法进行配置：

    - (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation NS_DEPRECATED_IOS(2_0, 6_0) __TVOS_PROHIBITED;


在iOS6之后，这个方法被NS_DEPRECATED_IOS，也就是废弃掉了。废弃了这个方法，苹果相应的也给出了新的方法来代替：

    // New Autorotation support.
    - (BOOL)shouldAutorotate NS_AVAILABLE_IOS(6_0) __TVOS_PROHIBITED;
    - (UIInterfaceOrientationMask)supportedInterfaceOrientations NS_AVAILABLE_IOS(6_0) __TVOS_PROHIBITED;


我们可以看到iOS6之前是一个方法，在iOS6之后变成两个方法了，一个是是否旋转的方法，一个是支持的方向的方法。

## 实例一：

假设：我们ViewController是直接加载window的self.window.rootViewController上面的。代码如下：

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Override point for customization after application launch.
        self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
        ViewController *vc = [[ViewController alloc] init];
        self.window.rootViewController = vc;
        [self.window makeKeyAndVisible];
        return YES;
    }


如果我们要是想支持上面General里勾选的方向（竖屏、横屏向左已经横屏向右）该如何实现呢？首先，我们应该设置让他支持旋转，然后在设置支持的方向。代码如下：

    //支持旋转
    -(BOOL)shouldAutorotate{
       return YES;
    }
    //支持的方向
    - (UIInterfaceOrientationMask)supportedInterfaceOrientations {
        return UIInterfaceOrientationMaskAllButUpsideDown;
    }


其中UIInterfaceOrientationMask是一个枚举：

    typedef NS_OPTIONS(NSUInteger, UIInterfaceOrientationMask) {
        UIInterfaceOrientationMaskPortrait = (1 << UIInterfaceOrientationPortrait),
        UIInterfaceOrientationMaskLandscapeLeft = (1 << UIInterfaceOrientationLandscapeLeft),
        UIInterfaceOrientationMaskLandscapeRight = (1 << UIInterfaceOrientationLandscapeRight),
        UIInterfaceOrientationMaskPortraitUpsideDown = (1 << UIInterfaceOrientationPortraitUpsideDown),
        UIInterfaceOrientationMaskLandscape = (UIInterfaceOrientationMaskLandscapeLeft | UIInterfaceOrientationMaskLandscapeRight),
        UIInterfaceOrientationMaskAll = (UIInterfaceOrientationMaskPortrait | UIInterfaceOrientationMaskLandscapeLeft | UIInterfaceOrientationMaskLandscapeRight | UIInterfaceOrientationMaskPortraitUpsideDown),
        UIInterfaceOrientationMaskAllButUpsideDown = (UIInterfaceOrientationMaskPortrait | UIInterfaceOrientationMaskLandscapeLeft | UIInterfaceOrientationMaskLandscapeRight),
    } __TVOS_PROHIBITED;


可以根据自己的需求来选择。上面我们说了假设这个条件，如果rootViewController上导航，我们直接在ViewController里面设置，这个方法就不灵了。（大家可以自己测试一下）

## 实例二：

为什么是导航上面的方法就不灵了呢？原因很简单，我们没有设置导航支持的方向。别忘了UINavigationController也是UIViewController的子类。需要受到同样的待遇的。

如何设置呢？我们可以创建一个UINavigationController的子类，假设叫GGPublicNavigationViewController。然后，我们在GGPublicNavigationViewController.m文件里面也实现着两个方法：

    //支持旋转
    -(BOOL)shouldAutorotate{
       return YES;
    }
    //支持的方向
    - (UIInterfaceOrientationMask)supportedInterfaceOrientations {
        return UIInterfaceOrientationMaskAllButUpsideDown;
    }


这样设置之后，即使我们push进去的UIViewController没有实现上面的连个方法，也是可以支持横屏的。也就是说，我们push的所有都支持横屏。这个做法是不是很暴力！

## 实例三：

有些童鞋会问了，如何控制每个界面支持的方向呢？这也是可以办到的，在GGPublicNavigationViewController不能写死支持哪个。我们可以这么写：

    -(BOOL)shouldAutorotate{
        return [self.topViewController shouldAutorotate];
    }
    //支持的方向
    - (UIInterfaceOrientationMask)supportedInterfaceOrientations {
        return [self.topViewController supportedInterfaceOrientations];;
    }


self.topViewController是当前导航显示的UIViewController，这样就可以控制每个UIViewController所支持的方向啦！

> 原文来自:http://www.superqq.com/blog/2015/12/07/ios9-interface-orientation/

