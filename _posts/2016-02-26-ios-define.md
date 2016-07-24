---
layout: post
title: "iOS 开发常用宏"
subtitle: "Macro definition is commonly used in ios development"
date: 2016-02-26 
author: "Alpaca"
categories: ios
tags:
    - iOS
    - Dev
---

    // App Frame
    #define Application_Frame       [[UIScreen mainScreen] applicationFrame]
    
    // App Frame Height Width
    #define App_Frame_Height        [[UIScreen mainScreen] applicationFrame].size.height
    #define App_Frame_Width         [[UIScreen mainScreen] applicationFrame].size.width
    
    // MainScreen Height Width
    #define Main_Screen_Height      [[UIScreen mainScreen] bounds].size.height
    #define Main_Screen_Width       [[UIScreen mainScreen] bounds].size.width


---

    // View 坐标(x,y)和宽高(width,height)
    #define X(v)                    (v).frame.origin.x
    #define Y(v)                    (v).frame.origin.y
    
    #define WIDTH(v)                (v).frame.size.width
    #define HEIGHT(v)               (v).frame.size.height
    
    #define MinX(v)                 CGRectGetMinX((v).frame)
    #define MinY(v)                 CGRectGetMinY((v).frame)
    
    #define MidX(v)                 CGRectGetMidX((v).frame)
    #define MidY(v)                 CGRectGetMidY((v).frame)
    
    #define MaxX(v)                 CGRectGetMaxX((v).frame)
    #define MaxY(v)                 CGRectGetMaxY((v).frame)


---

    // 系统控件默认高度
    #define StatusBarHeight        (20.f)
    #define TopBarHeight           (44.f)
    #define BottomBarHeight        (49.f)
    #define CellDefaultHeight      (44.f)
    #define EnglishKeyboardHeight  (216.f)
    #define ChineseKeyboardHeight  (252.f)


---

    // 沙盒路径 
    #define PATH_OF_APP_HOME    NSHomeDirectory()
    #define PATH_OF_TEMP        NSTemporaryDirectory()
    #define PATH_OF_DOCUMENT    [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0]


---

    // 加载图片
    #define PNGIMAGE(NAME)          [UIImage imageWithContentsOfFile:[[NSBundle mainBundle] pathForResource:(NAME) ofType:@"png"]]
    #define JPGIMAGE(NAME)          [UIImage imageWithContentsOfFile:[[NSBundle mainBundle] pathForResource:(NAME) ofType:@"jpg"]]
    #define IMAGE(NAME, EXT)        [UIImage imageWithContentsOfFile:[[NSBundle mainBundle] pathForResource:(NAME) ofType:(EXT)]]


---

    // 颜色(RGB)
    #define RGBCOLOR(r, g, b)       [UIColor colorWithRed:(r)/255.0f green:(g)/255.0f blue:(b)/255.0f alpha:1]
    #define RGBACOLOR(r, g, b, a)   [UIColor colorWithRed:(r)/255.0f green:(g)/255.0f blue:(b)/255.0f alpha:(a)]
    // 随机颜色
    #define RANDOM_UICOLOR     [UIColor colorWithRed:arc4random_uniform(256) / 255.0 green:arc4random_uniform(256) / 255.0 blue:arc4random_uniform(256) / 255.0 alpha:1]


---

    // View 圆角和加边框
    #define ViewBorderRadius(View, Radius, Width, Color)\
                                    \
                                    [View.layer setCornerRadius:(Radius)];\
                                    [View.layer setMasksToBounds:YES];\
                                    [View.layer setBorderWidth:(Width)];\
                                    [View.layer setBorderColor:[Color CGColor]]
    
    // View 圆角
    #define ViewRadius(View, Radius)\
                                    \
                                    [View.layer setCornerRadius:(Radius)];\
                                    [View.layer setMasksToBounds:YES]


---

    // 当前语言
    #define CURRENTLANGUAGE         ([[NSLocale preferredLanguages] objectAtIndex:0])


---

    // 本地化字符串
    /** NSLocalizedString宏做的其实就是在当前bundle中查找资源文件名“Localizable.strings”(参数:键＋注释) */
    #define LocalString(x, ...)     NSLocalizedString(x, nil)
    /** NSLocalizedStringFromTable宏做的其实就是在当前bundle中查找资源文件名“xxx.strings”(参数:键＋文件名＋注释) */
    #define AppLocalString(x, ...)  NSLocalizedStringFromTable(x, @"someName", nil)


---

    // 时间间隔 
    #define kHUDDuration            (1.f)
    // 一天的秒数 
    #define SecondsOfDay            (24.f * 60.f * 60.f)
    // 秒数
    #define Seconds(Days)           (24.f * 60.f * 60.f * (Days))
    // 一天的毫秒数
    #define MillisecondsOfDay       (24.f * 60.f * 60.f * 1000.f)
    // 毫秒数
    #define Milliseconds(Days)      (24.f * 60.f * 60.f * 1000.f * (Days))


---

    // block self
    #define BlockWeakObject(o) __typeof(o) __weak
    #define BlockWeakSelf BlockWeakObject(self)

