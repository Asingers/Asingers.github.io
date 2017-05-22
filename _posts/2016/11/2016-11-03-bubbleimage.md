---
layout: post
title: "气泡图片"
date: 2016-11-03
author: "Alpaca"
subtitle: "给ImageView加Layer"
catalog: true
categories: ios
tags:
   - iOS
   
---

![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-11-03-16%3A26%3A22.jpg)

准备一张这种气泡效果的背景图，然后将这个气泡图做成一个layer实例，并且通过contentCenter或者contentRect拉伸至我们需要展示的UIImageView大小  

将做好的layer实例赋给UIImageView

将imageview赋上图片

    CGRect frame = CGRectMake(20, 100, 300, 300);
    
    UIImageView *image = [[UIImageView alloc] initWithFrame:frame];
    
    CAShapeLayer *layer = [CAShapeLayer layer];
    
    layer.frame = image.bounds;
    layer.contents = (id)[UIImage imageNamed:@"chat.png"].CGImage;
    layer.contentsCenter = CGRectMake(0.5, 0.5, 0.1, 0.1);
    layer.contentsScale = [UIScreen mainScreen].scale;
    
    image.layer.mask = layer;
    image.layer.frame = image.frame;
    image.image = [UIImage imageNamed:@"1.jpg"];
    [self.view addSubview:image];
    
Like this ⤵️:  

![](http://7xqmgj.com1.z0.glb.clouddn.com/2016-11-03-16%3A27%3A46.jpg)