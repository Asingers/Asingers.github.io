---
layout: post
title: "iOS开发-检测用户截屏,并获取所截图片"
date: 2016-04-13
author: "Asingers"
subtitle: "实用案例"
categories: iOS
tags:
   - iOS
---

**微信,QQ,淘宝,微博...可以检测到用户截屏行为（Home + Power），并在稍后点击附加功能按钮时询问用户是否要发送刚才截屏的图片，这个用户体验非常好。试着探究一下如何做到这一点**

**
**

在iOS7之前, 如果用户截屏，系统会自动取消屏幕上的所有 touch 事件，（使用**touchesCancelled:withEvent**: 这个方法）那么我们就可以检测这个方法的调用，然后加载本地最新图片再加以判断来实现我们的目的。但在 iOS 7 之后，截屏不再会取消屏幕的 touch 事件，所以导致了 Snapchat 和 Facebook Poke 之类的应用在 iOS 7 刚发布时依赖于系统这个行为的功能受到影响。

如果不采取任何新措施, 我们可以让应用启动后在后台循环检测相册内最新一张照片，看它的是否符合截屏的特征。这种方法可行，但这是个笨方法，需要用户允许你的程序访问相册才可以，并且一直在后台循环会消耗更多的系统资源。

当然, 苹果封闭了一些东西, 肯定也会给你开放其他东西, 不会让你走上绝路的。

iOS7提供一个崭新的推送方法：**UIApplicationUserDidTakeScreenshotNotification**。只要像往常一样订阅即可知道什么时候截图了。
注意：UIApplicationUserDidTakeScreenshotNotification**将会在截图完成之后显示**。现在在截图截取之前无法得到通知。

希望苹果会在iOS8当中增加 UIApplicationUserWillTakeScreenshotNotification。（只有did, 没有will显然不是苹果的风格...）

**Demo 稍后放出**
# 一。注册通知:


    //注册通知
        [[NSNotificationCenter defaultCenter] addObserver:self
                                                 selector:@selector(userDidTakeScreenshot:)
                                                     name:UIApplicationUserDidTakeScreenshotNotification object:nil];





# 二。监听截屏:

执行操作, 也就是实现上面通知对应的响应函数  －－ userDidTakeScreenshot


    //截屏响应
    - (void)userDidTakeScreenshot:(NSNotification *)notification
    {
        NSLog(@"检测到截屏");
        
        //人为截屏, 模拟用户截屏行为, 获取所截图片
        UIImage *image_ = [self imageWithScreenshot];
        
        //添加显示
        UIImageView *imgvPhoto = [[UIImageView alloc]initWithImage:image_];
        imgvPhoto.frame = CGRectMake(self.window.frame.size.width/2, self.window.frame.size.height/2, self.window.frame.size.width/2, self.window.frame.size.height/2);
        
        //添加边框
        CALayer * layer = [imgvPhoto layer];
        layer.borderColor = [
                             [UIColor whiteColor] CGColor];
        layer.borderWidth = 5.0f;
        //添加四个边阴影
        imgvPhoto.layer.shadowColor = [UIColor blackColor].CGColor;
        imgvPhoto.layer.shadowOffset = CGSizeMake(0, 0);
        imgvPhoto.layer.shadowOpacity = 0.5;
        imgvPhoto.layer.shadowRadius = 10.0;
        //添加两个边阴影
        imgvPhoto.layer.shadowColor = [UIColor blackColor].CGColor;
        imgvPhoto.layer.shadowOffset = CGSizeMake(4, 4);
        imgvPhoto.layer.shadowOpacity = 0.5;
        imgvPhoto.layer.shadowRadius = 2.0;
    
        [self.window addSubview:imgvPhoto];
    }





我这里的 userDidTakeScreenshot 总共做了3件事

1.打印检测到截屏

2.获取截屏图片。调用[self imageWithScreenshot];这里的imageWithScreenshot是人为截屏, 模拟用户截屏操作, 获取截屏图片。

3.显示截屏图片, 以屏幕1/4大小显示在右下角, 并且加上白色边框和阴影效果突出显示。




# 三。获取截屏图片


    /**
     *  截取当前屏幕
     *
     *  @return NSData *
     */
    - (NSData *)dataWithScreenshotInPNGFormat
    {
        CGSize imageSize = CGSizeZero;
        UIInterfaceOrientation orientation = [UIApplication sharedApplication].statusBarOrientation;
        if (UIInterfaceOrientationIsPortrait(orientation))
            imageSize = [UIScreen mainScreen].bounds.size;
        else
            imageSize = CGSizeMake([UIScreen mainScreen].bounds.size.height, [UIScreen mainScreen].bounds.size.width);
        
        UIGraphicsBeginImageContextWithOptions(imageSize, NO, 0);
        CGContextRef context = UIGraphicsGetCurrentContext();
        for (UIWindow *window in [[UIApplication sharedApplication] windows])
        {
            CGContextSaveGState(context);
            CGContextTranslateCTM(context, window.center.x, window.center.y);
            CGContextConcatCTM(context, window.transform);
            CGContextTranslateCTM(context, -window.bounds.size.width * window.layer.anchorPoint.x, -window.bounds.size.height * window.layer.anchorPoint.y);
            if (orientation == UIInterfaceOrientationLandscapeLeft)
            {
                CGContextRotateCTM(context, M_PI_2);
                CGContextTranslateCTM(context, 0, -imageSize.width);
            }
            else if (orientation == UIInterfaceOrientationLandscapeRight)
            {
                CGContextRotateCTM(context, -M_PI_2);
                CGContextTranslateCTM(context, -imageSize.height, 0);
            } else if (orientation == UIInterfaceOrientationPortraitUpsideDown) {
                CGContextRotateCTM(context, M_PI);
                CGContextTranslateCTM(context, -imageSize.width, -imageSize.height);
            }
            if ([window respondsToSelector:@selector(drawViewHierarchyInRect:afterScreenUpdates:)])
            {
                [window drawViewHierarchyInRect:window.bounds afterScreenUpdates:YES];
            }
            else
            {
                [window.layer renderInContext:context];
            }
            CGContextRestoreGState(context);
        }
        
        UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
        UIGraphicsEndImageContext();
        
        return UIImagePNGRepresentation(image);
    }
    
    /**
     *  返回截取到的图片
     *
     *  @return UIImage *
     */
    - (UIImage *)imageWithScreenshot
    {
        NSData *imageData = [self dataWithScreenshotInPNGFormat];
        return [UIImage imageWithData:imageData];
    }








