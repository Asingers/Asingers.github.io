---
layout: post
title: Masonry与ScrollView使用注意
subtitle: 避免被坑哦
author: "Asingers"
categories: ios
header-mask: 0.3
tags: 
    - iOS
---
```

#import "ViewController.h"
#import <Masonry.h>
 
@interface ViewController ()
 
// scrollView
@property (nonatomic, strong) UIScrollView *scrollView;
// 约束参照视图,也是容器视图
@property (nonatomic, strong) UIView *contentView;
// 第一个测试view
@property (nonatomic, strong) UIView *oneView;
// 第二个测试view
@property (nonatomic, strong) UIView *twoView;
// 第三个测试view
@property (nonatomic, strong) UIView *threeView;
 
@end
 
@implementation ViewController
 
- (void)viewDidLoad {
    [super viewDidLoad];
    // 1. 添加scrollView
    [self.view addSubview:self.scrollView];
    // 2. 添加参照视图
    [self.scrollView addSubview:self.contentView];
    // 3. 添加第一个测试view
    [self.contentView addSubview:self.oneView];
    // 4. 添加第二个测试view
    [self.contentView addSubview:self.twoView];
    // 5. 添加第三个测试view
    [self.contentView addSubview:self.threeView];
}
 
- (void)viewDidLayoutSubviews {
    [super viewDidLayoutSubviews];
    // 布局子控件
    // 设置scrollView约束
    [self.scrollView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.edges.equalTo(self.view);
    }];
    // 设置参照视图的约束
    [self.contentView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.edges.equalTo(self.scrollView);
        make.width.equalTo(self.scrollView);
//        make.height.greaterThanOrEqualTo(@0.0f);
    }];
    // 第一个测试view的约束
    [self.oneView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.equalTo(self.contentView).offset(30);
        make.left.equalTo(self.contentView);
        make.width.mas_equalTo(200);
        make.height.mas_equalTo(300);
    }];
    // 第二个测试view的约束
    [self.twoView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.equalTo(self.oneView.mas_bottom).offset(50);
        make.right.equalTo(self.contentView);
        make.width.mas_equalTo(400);
        make.height.mas_equalTo(500);
    }];
    // 第三个测试view的约束
    [self.threeView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.top.equalTo(self.twoView.mas_bottom).offset(70);
        make.left.right.equalTo(self.contentView);
        make.height.mas_equalTo(300);
    }];
    // 最后设置最后一个view的与参照容器view的约束
    [self.contentView mas_makeConstraints:^(MASConstraintMaker *make) {
        make.bottom.equalTo(self.threeView.mas_bottom).offset(-10);
    }];
}
 
#pragma mark - getters
// scrollView
- (UIScrollView *)scrollView {
    if (_scrollView == nil) {
        _scrollView = [[UIScrollView alloc] init];
        _scrollView.backgroundColor = [UIColor grayColor];
    }
    return _scrollView;
}
// 约束参照视图
- (UIView *)contentView {
    if (_contentView == nil) {
        _contentView = [[UIView alloc] init];
        _contentView.backgroundColor = [UIColor yellowColor];
    }
    return _contentView;
}
// 第一个测试view
- (UIView *)oneView {
    if (_oneView == nil) {
        _oneView = [[UIView alloc] init];
        _oneView.backgroundColor = [UIColor redColor];
    }
    return _oneView;
}
// 第二个测试view
- (UIView *)twoView {
    if (_twoView == nil) {
        _twoView = [[UIView alloc] init];
        _twoView.backgroundColor = [UIColor blueColor];
    }
    return _twoView;
}
// 第三个测试view
- (UIView *)threeView {
    if (_threeView == nil) {
        _threeView = [[UIView alloc] init];
        _threeView.backgroundColor = [UIColor greenColor];
    }
    return _threeView;
}
 
@end

```