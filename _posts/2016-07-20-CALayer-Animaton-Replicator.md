---
layout: post
title: "CALayer Animation - Replicator Animation"
date: 2016-07-19
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_2016animation.jpeg"
author: "Asingers"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - Mac
   - iOS
   
---

先来看一下效果    

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_animation.gif" alt="" class="shadow"/>    

首先添加一个100x200的黑色View  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-20%20%E4%B8%8B%E5%8D%884.50.24.png" alt="" class="shadow"/>  

    
    _myView = [[UIView alloc]initWithFrame:CGRectMake(100, 100, 200, 100)];
    _myView.backgroundColor = [UIColor blackColor];
    [self.view addSubview:_myView];    
添加Layer

    CAReplicatorLayer *replicatorLayer = [[CAReplicatorLayer alloc]init];
    replicatorLayer.bounds = CGRectMake(_myView.frame.origin.x, _myView.frame.origin.y, _myView.width, _myView.height);
    replicatorLayer.anchorPoint = CGPointMake(0, 0);
    replicatorLayer.backgroundColor = [[UIColor lightGrayColor]CGColor];
    
    [_myView.layer addSublayer:replicatorLayer];
     
CAReplicatorLayer，它也是CALayer的子类，正如它的名称一样，CAReplicatorLayer可以对它自己的子Layer进行复制操作。创建了CAReplicatorLayer实例后，设置了它的尺寸大小、位置、锚点位置、背景色，并且将它添加到了_myView的Layer中:  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-20%20%E4%B8%8B%E5%8D%884.52.54.png" alt="" class="shadow"/>  

这里要啰嗦几句，Layer的默认锚点坐标是(0.5, 0.5)，也就是Layer的中心点位置，而Layer的position又是根据锚点计算的，所以如果你设置Layer的position属性为(10, 10)，就相当于设置了Layer的中心位置为(10, 10)，并不是你期望的左上角位置。所以如果Layer想使用它父视图的坐标位置，就需要将锚点位置设置为(0, 0)，这样一来Layer的position属性标识的就是Layer左上角的位置：

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_ReplicatorAnimation-3.png" alt="" class="shadow"/>  

    CALayer *mylayer = [[CALayer alloc]init];
    mylayer.bounds = CGRectMake(0, 0, 30, 90);
    mylayer.anchorPoint = CGPointMake(0, 0);
    mylayer.position = CGPointMake(_myView.frame.origin.x+10, _myView.frame.origin.y+100);
    mylayer.cornerRadius = 2;
    mylayer.backgroundColor = [[UIColor whiteColor]CGColor];
    [replicatorLayer addSublayer:mylayer];
    
通过上面的代码，再次创建了一个Layer，这次使用的是CALayer，因为我们只需要一个很普通的Layer，为其设置位置、尺寸、背景色、圆角属性，然后添加在replicatorLayer中：  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-20%20%E4%B8%8B%E5%8D%884.55.53.png" alt="" class="shadow"/>  

加动画让他动起来:  
首先我们创建了按Y轴移动的动画实例，然后设置了移动的目标位置，动画持续时间，重复次数设置为无限大。这里有一个属性大家可能比较陌生，那就是autoreverses，这个属性为Bool类型，设置为true时，开启自动反向执行动画，比如示例中的白色长方形的移动动画为向上移动50个像素，如过autoreverses设置为false，那么动画结束后，会根据重复次数，白色长方形重新回到初始位置，继续向上移动，如果autoreverses设置为true，则当动画结束后，白色长方形会继续向下移动至初始位置，然后再开始第二次的向上移动动画。

    CABasicAnimation *moveRectangle = [CABasicAnimation animationWithKeyPath:@"position"];
    moveRectangle.fromValue = [NSValue valueWithCGPoint:CGPointMake(110, 200)];
    moveRectangle.toValue = [NSValue valueWithCGPoint:CGPointMake(110, 130)];
    moveRectangle.duration = 0.7;
    moveRectangle.autoreverses = true;
    moveRectangle.repeatCount = HUGE;
   
    [mylayer addAnimation:moveRectangle forKey:nil];  
    
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_animation-1.gif" alt="" class="shadow"/>  

layer数量:

    replicatorLayer.instanceCount = 4;  

但是4个重叠在一起,我们看不出效果.显而易见，这是CAReplicatorLayer的能力了，这行代码的意思是将replicatorLayer的子Layer复制3份，复制Layer与原Layer的大小、位置、颜色、Layer上的动画等等所有属性都一模一样，所以这时编译运行代码我们看不到任何不同的效果，因为三个白色长方形是重合在一起的，所以我们需要设置每个白色长方形的间隔：

    replicatorLayer.instanceTransform = CATransform3DMakeTranslation(40, 0, 0);  
    
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_animation-2.gif" alt="" class="shadow"/>  

现在三个白色长方形的运动轨迹和时刻都是一直的，这显然不是我们想要的结果，我们需要三个白色长方形有上下起伏的视觉效果，所以我们继续添加一行代码：  

    replicatorLayer.instanceDelay = 0.3;  
    
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_animation-3.gif" alt="" class="shadow"/>  

显然我们只想显示replicatorLayer区域里的内容，我们并不想看到超出它边界的内容，所以我们再添加一行代码：  

        replicatorLayer.masksToBounds = YES;

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-20_animation-4.gif" alt="" class="shadow"/>  







  

    