---
layout: post
title: "纯代码创建UICollectionView步骤以及简单使用"
subtitle: "解读"
date: 2016-03-08
author: "Asingers"
categories: ios
tags:
    - iOS
---

UICollectionView主要用于瀑布流，由于一直接触较少，每次需要使用的时候都从网上翻阅资料，此次自己总结整理，以备不时之需。

- collectionView和tableView最大的不同之处就是需要自定义cell,所以第一步自定义collectionViewCell


.h文件

    #import <UIKit/UIKit.h>
    
    @interface MyCollectionViewCell : UICollectionViewCell
    
    @property (strong, nonatomic) UIImageView *topImage;
    
    @property (strong, nonatomic) UILabel *botlabel;
    
    @end


.m文件

    #import "MyCollectionViewCell.h"
    
    @implementation MyCollectionViewCell
    
    - (id)initWithFrame:(CGRect)frame
    {
        self = [super initWithFrame:frame];
        if (self)
        {
            _topImage  = [[UIImageView alloc] initWithFrame:CGRectMake(10, 0, 70, 70)];
            _topImage.backgroundColor = [UIColor redColor];
            [self.contentView addSubview:_topImage];
    
            _botlabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 80, 70, 30)];
            _botlabel.textAlignment = NSTextAlignmentCenter;
            _botlabel.textColor = [UIColor blueColor];
            _botlabel.font = [UIFont systemFontOfSize:15];
            _botlabel.backgroundColor = [UIColor purpleColor];
            [self.contentView addSubview:_botlabel];
        }
    
        return self;
    }
    
    @end


在viewController中实现collectionView的三个协议

> 
> UICollectionViewDataSource,UICollectionViewDelegate,UICollectionViewDelegateFlowLayout
> 

具体实例化步骤均在代码中有注释，如下



    #import "CollectionViewController.h"
    #import "MyCollectionViewCell.h"
    
    @interface CollectionViewController ()<UICollectionViewDataSource,UICollectionViewDelegate,UICollectionViewDelegateFlowLayout>
    {
        UICollectionView *mainCollectionView;
    }
    
    @end
    
    @implementation CollectionViewController
    
    - (void)viewDidLoad {
        [super viewDidLoad];
        // Do any additional setup after loading the view.
        self.view.backgroundColor = [UIColor whiteColor];
    
        //1.初始化layout
        UICollectionViewFlowLayout *layout = [[UICollectionViewFlowLayout alloc] init];
        //设置collectionView滚动方向
    //    [layout setScrollDirection:UICollectionViewScrollDirectionHorizontal];
        //设置headerView的尺寸大小
        layout.headerReferenceSize = CGSizeMake(self.view.frame.size.width, 100);
        //该方法也可以设置itemSize
        layout.itemSize =CGSizeMake(110, 150);
    
        //2.初始化collectionView
        mainCollectionView = [[UICollectionView alloc] initWithFrame:self.view.bounds collectionViewLayout:layout];
        [self.view addSubview:mainCollectionView];
        mainCollectionView.backgroundColor = [UIColor clearColor];
    
        //3.注册collectionViewCell
        //注意，此处的ReuseIdentifier 必须和 cellForItemAtIndexPath 方法中 一致 均为 cellId
        [mainCollectionView registerClass:[MyCollectionViewCell class] forCellWithReuseIdentifier:@"cellId"];
    
        //注册headerView  此处的ReuseIdentifier 必须和 cellForItemAtIndexPath 方法中 一致  均为reusableView
        [mainCollectionView registerClass:[UICollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"reusableView"];
    
        //4.设置代理
        mainCollectionView.delegate = self;
        mainCollectionView.dataSource = self;
    }
    
    
    #pragma mark collectionView代理方法
    //返回section个数
    - (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView
    {
        return 3;
    }
    
    //每个section的item个数
    - (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section
    {
        return 9;
    }
    
    - (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath
    {
    
        MyCollectionViewCell *cell = (MyCollectionViewCell *)[collectionView dequeueReusableCellWithReuseIdentifier:@"cellId" forIndexPath:indexPath];
    
        cell.botlabel.text = [NSString stringWithFormat:@"{百分号ld,百分号ld}",(long)indexPath.section,(long)indexPath.row];
    
        cell.backgroundColor = [UIColor yellowColor];
    
        return cell;
    }
    
    //设置每个item的尺寸
    - (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath
    {
        return CGSizeMake(90, 130);
    }
    
    //footer的size
    //- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout referenceSizeForFooterInSection:(NSInteger)section
    //{
    //    return CGSizeMake(10, 10);
    //}
    
    //header的size
    //- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout referenceSizeForHeaderInSection:(NSInteger)section
    //{
    //    return CGSizeMake(10, 10);
    //}
    
    //设置每个item的UIEdgeInsets
    - (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout insetForSectionAtIndex:(NSInteger)section
    {
        return UIEdgeInsetsMake(10, 10, 10, 10);
    }
    
    //设置每个item水平间距
    - (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout minimumInteritemSpacingForSectionAtIndex:(NSInteger)section
    {
        return 10;
    }
    
    
    //设置每个item垂直间距
    - (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout minimumLineSpacingForSectionAtIndex:(NSInteger)section
    {
        return 15;
    }
    
    
    //通过设置SupplementaryViewOfKind 来设置头部或者底部的view，其中 ReuseIdentifier 的值必须和 注册是填写的一致，本例都为 “reusableView”
    - (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath
    {
        UICollectionReusableView *headerView = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSectionHeader withReuseIdentifier:@"reusableView" forIndexPath:indexPath];
        headerView.backgroundColor =[UIColor grayColor];
        UILabel *label = [[UILabel alloc] initWithFrame:headerView.bounds];
        label.text = @"这是collectionView的头部";
        label.font = [UIFont systemFontOfSize:20];
        [headerView addSubview:label];
        return headerView;
    }
    
    //点击item方法
    - (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath
    {
        MyCollectionViewCell *cell = (MyCollectionViewCell *)[collectionView cellForItemAtIndexPath:indexPath];
        NSString *msg = cell.botlabel.text;
        NSLog(@"%@",msg);
    }
    
    - (void)didReceiveMemoryWarning {
        [super didReceiveMemoryWarning];
        // Dispose of any resources that can be recreated.
    }
    
    /*
    #pragma mark - Navigation
    
    // In a storyboard-based application, you will often want to do a little preparation before navigation
    - (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
        // Get the new view controller using [segue destinationViewController].
        // Pass the selected object to the new view controller.
    }
    */
    
    @end


完成以上两步，效果图如下：

![demo.gif](http://upload-images.jianshu.io/upload_images/327661-1dd9b88e4026599e.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

demo.gif


