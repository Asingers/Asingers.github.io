---
layout: post
title:  "利用长按手势移动TableViewCells"
date:   2016-01-26 15:38:45
author:     "Alpaca"
comments: true
categories: iOS
tags:
     - TableView
---

本文译自：[Cookbook: Moving Table View Cells with a Long Press Gesture](http://www.raywenderlich.com/63089/cookbook-moving-table-view-cells-with-a-long-press-gesture)

目录：

- 你需要什么？
- 如何做？
- 如何将其利用至UICollectionView上？
- 何去何从？


本次的 cookbook-style 教程中介绍如何通过长按手势来移动 table view中的cell，这种操作方式就像苹果自家的天气 App 一样。

你可以直接把本文中的到吗添加到你的工程中，或者将其添加到我为你创建好的[starter project](https://github.com/moayes/UDo/tree/UDo.Starter)中，也可以下载本文的[完整示例工程](https://github.com/moayes/UDo/tree/master)。

## 你需要什么？

- UILongGestureRecognizer
- UITableView (可以用 UICollectionView 替代之)
- UITableViewController (可以用 UIViewController 或 UICollectionViewController 替代之)
- 5 分钟。


## 如何做？

首先给 table view 添加一个`UILongGestureRecognizer`。可以在 table view controller 的`viewDidLoad`方法中添加。


    
    UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc]
    
    initWithTarget:selfaction:@selector(longPressGestureRecognized:)];
    
    [self.tableViewaddGestureRecognizer:longPress];
    


记者为 gesture recognizer 添加 action 方法。该方法首先应该获取到在 table view 中长按的位置，然后找出这个位置对应的 cell 的 index。记住：这里获取到的 index path 有可能为 nil(例如，如果用户长按在 table view的section header上)。



    
    - (IBAction)longPressGestureRecognized:(id)sender {
    
    UILongPressGestureRecognizer *longPress = (UILongPressGestureRecognizer *)sender;
    
    UIGestureRecognizerState state = longPress.state;
    
    CGPointlocation = [longPress locationInView:self.tableView];
    
    NSIndexPath*indexPath = [self.tableViewindexPathForRowAtPoint:location];
    
    // More coming soon...
    
    }
    


接着你需要处理`UIGestureRecognizerStateBegan`分支。如果获取到一个有效的 index path(non-nil)，就去获取对应的`UITableViewCell`，并利用一个 helper 方法获取这个 table view cell 的 snapshot  view。然后将这个 snapshot  view 添加到 table view 中，并将其 center 到对应的 cell上。

为了更好的用户体验，以及更自然的效果，在这里我把原始 cell 的背景设置为黑色，并给 snapshot  view 增加淡入效果，让 snapshot view 比 原始 cell 稍微大一点，将它的Y坐标偏移量与手势的位置的Y轴对齐。这样处理之后，cell 就像从 table view 中跳出，然后浮在上面，并捕捉到用户的手指。

    staticUIView*snapshot =nil;///< A snapshot of the row user is moving.
    
    staticNSIndexPath*sourceIndexPath =nil;///< Initial index path, where gesture begins.
    
    switch(state) {
    
    caseUIGestureRecognizerStateBegan: {
    
    if(indexPath) {
    
    sourceIndexPath = indexPath;
    
    UITableViewCell*cell = [self.tableViewcellForRowAtIndexPath:indexPath];
    
    // Take a snapshot of the selected row using helper method.
    
    snapshot = [selfcustomSnapshotFromView:cell];
    
    // Add the snapshot as subview, centered at cell's center...
    
    __blockCGPointcenter = cell.center;
    
    snapshot.center= center;
    
    snapshot.alpha=0.0;
    
    [self.tableViewaddSubview:snapshot];
    
    [UIViewanimateWithDuration:0.25animations:^{
    
    // Offset for gesture location.
    
    center.y= location.y;
    
    snapshot.center= center;
    
    snapshot.transform= CGAffineTransformMakeScale(1.05,1.05);
    
    snapshot.alpha=0.98;
    
    // Black out.
    
    cell.backgroundColor= [UIColorblackColor];
    
    } completion:nil];
    
    }
    
    break;
    
    }
    
    // More coming soon...
    
    }
    


将下面的方法添加到 .m 文件的尾部。该方法会根据传入的 view，返回一个对应的 snapshot view。
    
    - (UIView*)customSnapshotFromView:(UIView*)inputView {
    
    UIView*snapshot = [inputView snapshotViewAfterScreenUpdates:YES];
    
    snapshot.layer.masksToBounds=NO;
    
    snapshot.layer.cornerRadius=0.0;
    
    snapshot.layer.shadowOffset= CGSizeMake(-5.0,0.0);
    
    snapshot.layer.shadowRadius=5.0;
    
    snapshot.layer.shadowOpacity=0.4;
    
    returnsnapshot;
    
    }
    


当手势移动的时候，也就是`UIGestureRecognizerStateChanged`分支，此时需要移动 snapshot view(只需要设置它的 Y 轴偏移量即可)。如果手势移动的距离对应到另外一个 index path，就需要告诉 table view，让其移动 rows。同时，你需要对 data source 进行更新：
    
    caseUIGestureRecognizerStateChanged: {
    
    CGPointcenter = snapshot.center;
    
    center.y= location.y;
    
    snapshot.center= center;
    
    // Is destination valid and is it different from source?
    
    if(indexPath && ![indexPath isEqual:sourceIndexPath]) {
    
    // ... update data source.
    
    [self.objectsexchangeObjectAtIndex:indexPath.rowwithObjectAtIndex:sourceIndexPath.row];
    
    // ... move the rows.
    
    [self.tableViewmoveRowAtIndexPath:sourceIndexPath toIndexPath:indexPath];
    
    // ... and update source so it is in sync with UI changes.
    
    sourceIndexPath = indexPath;
    
    }
    
    break;
    
    }
    
    // More coming soon...
    


最后，当手势结束或者取消时，table view 和 data source 都是最新的。你所需要做的事情就是将 snapshot view 从 table view 中移除，并把 cell 的背景色还原为白色。

为了提升用户体验，我们将 snapshot view 淡出，并让其尺寸变小至与 cell 一样。这样看起来就像把 cell 放回原处一样。
    
    default: {
    
    // Clean up.
    
    UITableViewCell*cell = [self.tableViewcellForRowAtIndexPath:sourceIndexPath];
    
    [UIViewanimateWithDuration:0.25animations:^{
    
    snapshot.center= cell.center;
    
    snapshot.transform= CGAffineTransformIdentity;
    
    snapshot.alpha=0.0;
    
    // Undo the black-out effect we did.
    
    cell.backgroundColor= [UIColorwhiteColor];
    
    } completion:^(BOOLfinished) {
    
    [snapshot removeFromSuperview];
    
    snapshot =nil;
    
    }];
    
    sourceIndexPath =nil;
    
    break;
    
    }
    


就这样，搞定了！编译并运行程序，现在可以通过长按手势对 tableview cells重新排序！

你可以在 GitHub 上下载到[完整的示例工程](https://github.com/moayes/UDo/tree/master)。

## 如何将其利用至UICollectionView上？

假设你已经有一个示例工程使用了`UICollectionView`，那么你可以很简单的就使用上本文之前介绍的代码。所需要做的事情就是用`self.collectionView`替换掉`self.tableView`，并更新一下获取和移动`UICollectionViewCell`的调用方法。

这里有个练习，从 GitHub上 checkout 出[UICollectionView 的starter project](https://github.com/moayes/UDo/tree/UDo.UICollectionView.Starter)，然后将 tap-和-hold 手势添加进去以对 cells 进行重排。这里可以下载到已经实现好了[工程](https://github.com/moayes/UDo/tree/UDo.UICollectionView)。

## 何去何从？

我们深深的希望你喜欢这篇文章！如果以后你想要看到类似更多 cookbook-style 的文章，可以告诉我们。

另外，如果你愿意观看本文的视频版，可以来[这里](http://www.raywenderlich.com/68102/video-tutorial-table-views-moving-rows)看。


> 原文来自 http://beyondvincent.com/2014/03/26/2014-03-26-cookbook-moving-table-view-cells-with-a-long-press-gesture/


