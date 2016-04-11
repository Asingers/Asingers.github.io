---
layout: post
title: "iOS横竖屏控制和事件处理"
date: 2016-04-11
author: "Asingers"
subtitle: "又一篇"
categories: iOS
tags:
   - iOS
---


首先，确保App本身应该允许转屏切换：

<img src="http://simayang.com/wp-content/uploads/2015/11/屏幕快照-2015-11-07-下午2.31.19.png" alt="" class="shadow"/>

再次，我的App里面都是走UINavigationController进行界面push切换的，所以首先创建一个UINavigationController的子类，并设定允许转屏：

    @implementation AppExtendNavigationController
    
    - (void)viewDidLoad {
        [super viewDidLoad];
        // Do any additional setup after loading the view.
    }
    
    - (void)didReceiveMemoryWarning {
        [super didReceiveMemoryWarning];
        // Dispose of any resources that can be recreated.
    }
    
    #pragma mark 转屏方法重写
    -(UIInterfaceOrientationMask)supportedInterfaceOrientations
    {
        return [self.viewControllers.lastObject supportedInterfaceOrientations];
    }
    
    -(UIInterfaceOrientation)preferredInterfaceOrientationForPresentation
    {
        return [self.viewControllers.lastObject preferredInterfaceOrientationForPresentation];
    }
    
    -(BOOL)shouldAutorotate{
        return self.visibleViewController.shouldAutorotate;
    }


最后，在你不想转屏切换的ViewController上重写以下方法：

    #pragma mark 转屏方法 不允许转屏
    -(UIInterfaceOrientationMask)supportedInterfaceOrientations
    {
        return UIInterfaceOrientationMaskPortrait ;
    }
    
    - (BOOL)shouldAutorotate
    {
        return NO;
    }
    
    -(UIInterfaceOrientation)preferredInterfaceOrientationForPresentation
    {
        return UIInterfaceOrientationPortrait;
    }
    
    - (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation{
        return NO;
    }


在你想转屏切换的ViewController上可以照这样重写（允许左右横屏以及竖屏）：

    - (BOOL)shouldAutorotate {
        return YES;
    }
    
    -(UIInterfaceOrientationMask)supportedInterfaceOrientations
    {
        return UIInterfaceOrientationMaskAll;
    }
    
    - (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation
    {
        return UIInterfaceOrientationPortrait;
    }
    
    - (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
    {
        return YES;
    }


另外，在ViewController中对于转屏事件可以参见下面的方法进行捕获：

    - (void)viewWillTransitionToSize:(CGSize)size withTransitionCoordinator:(id<UIViewControllerTransitionCoordinator>)coordinator
    {
        [super viewWillTransitionToSize:size withTransitionCoordinator:coordinator];
        [coordinator animateAlongsideTransition:^(id<UIViewControllerTransitionCoordinatorContext> context) {
            //计算旋转之后的宽度并赋值
            CGSize screen = [UIScreen mainScreen].bounds.size;
            //界面处理逻辑
            self.lineChartView.frame = CGRectMake(0, 30, screen.width, 200.0);
            //动画播放完成之后
            if(screen.width > screen.height){
                NSLog(@"横屏");
            }else{
                NSLog(@"竖屏");
            }
    
        } completion:^(id<UIViewControllerTransitionCoordinatorContext> context) {
            NSLog(@"动画播放完之后处理");
        }];
    }


*区分当前屏幕是否为横竖屏的状态，其实通过判断当前屏幕的宽高来决定是不是横屏或者竖屏：*

*竖屏时：宽<高*

*横屏时：宽>高*

*以上在IOS8、9中测试通过*






