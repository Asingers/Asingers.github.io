---
layout: post
title: "iOS9-SFSafariViewController"
date: 2016-04-21
author: "Asingers"
catalog: true
subtitle: "iOS9 引入"
categories: ios
tags:
   - iOS
---

   有时候需要在App内部打开一个网页，例如为了展示公司官网，产品列表信息，Facebook，微博等。以前都是使用 UIWebView，iOS 8引入了WKWebView。但他们都存在各自的一些问题。
   
#### **UIWebView**

1. 始祖级别，支持的iOS版本比较多
2. 可支持打开URL，包括各种URL模式，例如 Https，FTP等
3. 可支持打开各种不同文件格式，例如 txt，docx，ppt,，音视频文件等，很多文档阅读器会经常使用这个特性，感兴趣的可以查一下Apple的文档，支持的格式还是挺多，只是不同iOS 版本的支持程度不太一样，使用时请多留意测试确认~
4. 占用内存比较多，尤其是网页中包含比较多CSS+DIV之类内容时，很容易出现内存警告（Memory Warning）
5. 效率低，不灵活，尤其是和 JavaScript交互时
6. 无法清除本地存储数据（Local Storage）
7. 代理（delegate）之间的回调比较麻烦，提供的内容比较低级，尤其是UI部分。如果想自己定制一个类似 Safari 的内嵌浏览器（Browser），那就坑爹无极限了，例如我们PDF Reader系列中的内嵌Browser，自己手动模拟实现Tab切换，底部Tool及各种Menu等，说多了都是泪...

#### **WKWebView**

1. iOS 8引入的，比较年轻
2. 在内存和执行效率上要比UIWebView高很多
3. 开放度较高但据说Bug成吨
4. 类似UIWebView，UI定制比较麻烦

#### **SFSafariViewController**
1. iOS 9引入，更加年轻，意味着是Apple的新菜，总是有什么优势的
2. 也是用来显示网页内容的
3. 这是一个特殊的View Controller，而不是一个单独的 View，和前面两个的区别
4. 在当前App中使用Safari的UI框架展现Web内容，包括相同的地址栏，工具栏等，类似一个内置于App的小型Safari
5. 共享Safari的一些便利特性，包括：相似的用户体验，和Safari共享Cookie，iCloud Web表单数据，密码、证书自动填充，Safari阅读器（Safari Reader）
6. 可定制性比较差，甚至连地址栏都是不可编辑的，只能在init的时候，传入一个URL来指定网页的地址

如果你的App需要显示网页，但是又不想自己去定制浏览器界面的话，可以考虑用SFSafariViewController来试试。从好的方面看，SFSafariViewController也去掉了从App中跳转到Safari的撕裂感，不同App之间切换总是让人感觉麻烦和不舒服。
演示:
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/SFSafari.gif" alt="" class="shadow"/>

SFSafariViewController 的接口比较少，就不再继续一一列举了。另外一个定制功能在于SFSafariViewControllerDelegate里面的一个方法：

	-(NSArray<UIActivity *> *)safariViewController:(SFSafariViewController *)controller activityItemsForURL:(NSURL *)URL title:(nullable NSString *)title;

这个代理会在用户点击动作（Action）按钮（底部工具栏中间的按钮）的时候调用，可以传入UIActivity的数组，创建添加一些自定义的各类插件式的服务，比如分享到微信，微博什么的。

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/sf.png" alt="" class="shadow"/>

几行代码就可以搞定:

	服务,代理
	@import SafariServices;
	@interface MyViewController ()<SFSafariViewControllerDelegate>

	为控件添加方法showSF,并实现
    	
	-(void)showSF{

    NSString *urlString = @"http://www.baidu.com";
    
    SFSafariViewController *sfViewControllr = [[SFSafariViewController alloc] initWithURL:[NSURL URLWithString:urlString]];
    sfViewControllr.delegate = self;
    
    	[self presentViewController:sfViewControllr animated:YES completion:^{
        
    }];

	}
	- (void)safariViewControllerDidFinish:(nonnull SFSafariViewController *)controller
	{
    	[controller dismissViewControllerAnimated:YES completion:nil];
	}


