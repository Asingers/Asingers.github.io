---
layout: post
title: "iOS为UIView添加虚线边框"
date: 2016-06-17
author: "Alpaca"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
---

这玩意不经常用,但是属性不好记得住,做个笔记整理一下.
效果如下:  
<img src="http://ww2.sinaimg.cn/large/6a20008ejw1f4y9y5tbygj207a074wee.jpg" alt="" class="shadow"/>  

主要代码:  
可通过修改UIBezierPath来改变虚线框的路径。如果想把边框绘制成实线，可将borderLayer.lineDashPattern置为nil即可。

```
CGSize screenSize = [UIScreen mainScreen].bounds.size;
CGFloat viewWidth = 200;
CGFloat viewHeight = 200;
UIView *view = [[UIView alloc] initWithFrame:CGRectMake((screenSize.width - viewWidth)/2, (screenSize.height - viewHeight) / 2, viewWidth, viewHeight)];
view.backgroundColor = [UIColor colorWithWhite:0.9 alpha:1];
view.layer.cornerRadius = CGRectGetWidth(view.bounds)/2;
CAShapeLayer *borderLayer = [CAShapeLayer layer];
borderLayer.bounds = CGRectMake(0, 0, viewWidth, viewHeight);
borderLayer.position = CGPointMake(CGRectGetMidX(view.bounds), CGRectGetMidY(view.bounds));

//    borderLayer.path = [UIBezierPath bezierPathWithRect:borderLayer.bounds].CGPath;
borderLayer.path = [UIBezierPath bezierPathWithRoundedRect:borderLayer.bounds cornerRadius:CGRectGetWidth(borderLayer.bounds)/2].CGPath;
borderLayer.lineWidth = 1. / [[UIScreen mainScreen] scale];
//虚线边框
borderLayer.lineDashPattern = @[@8, @8];
//实线边框
//    borderLayer.lineDashPattern = nil;
borderLayer.fillColor = [UIColor clearColor].CGColor;
borderLayer.strokeColor = [UIColor redColor].CGColor;
[view.layer addSublayer:borderLayer];

[self.view addSubview:view];
```

