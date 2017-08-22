---
layout: post
title: iOS WKWebView
subtitle: iOS 8 新推出的 WKWebView 组件
categories: ios
header-mask: 0.3
tags: 
    - iOS
    - Xcode
    - WKWebView

---

WKWebView 是苹果在 iOS 8 中引入的新组件，目的是给出一个新的高性能的 Web View 解决方案，摆脱过去 UIWebView 的老旧笨重特别是内存占用量巨大的问题。

苹果将 UIWebViewDelegate 与 UIWebView 重构成了[ 14 个类和 3 个协议](https://developer.apple.com/documentation/webkit)引入了不少新的功能和接口.

WKWebView 有以下几大主要进步：

 - 将浏览器内核渲染进程提取出 App，由系统进行统一管理，这减少了相当一部分的性能损失。
- js 可以直接使用已经事先注入 js runtime 的 js 接口给 Native 层传值，不必再通过苦逼的 iframe 制造页面刷新再解析自定义协议的奇怪方式。
- 支持高达 60 fps 的滚动刷新率，内置了手势探测。

#### 创建
	// 引入头文件
	#import <WebKit/WebKit.h>

	//添加代理
	@interface ViewController () <WKScriptMessageHandler>

	@property (nonatomic, strong) WKWebView *wkWebView;

	@end

	@implementation ViewController
	
	- (void)viewDidLoad {
    [super viewDidLoad];
    WKWebViewConfiguration *config = [[WKWebViewConfiguration alloc] init];
    config.preferences.minimumFontSize = 18;
    
    self.wkWebView = [[WKWebView alloc] initWithFrame:CGRectMake(0, 0, self.view.bounds.size.width, self.view.bounds.size.height/2) configuration:config];
    [self.view addSubview:self.wkWebView];
    
    // 加载 HTML
    NSString *filePath = [[NSBundle mainBundle] pathForResource:@"index" ofType:@"html"];
    NSURL *baseURL = [[NSBundle mainBundle] bundleURL];
    [self.wkWebView loadHTMLString:[NSString stringWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:nil] baseURL:baseURL];
    
    
    WKUserContentController *userCC = config.userContentController;
    //JS要想调用OC 就得添加处理脚本,也就是 JS 中的方法名要注册到 OC, 得让 OC 知道
    
    [userCC addScriptMessageHandler:self name:@"showMobile"];
    [userCC addScriptMessageHandler:self name:@"showName"];
    [userCC addScriptMessageHandler:self name:@"showSendMsg"];
	}
	
#### JS 调用 OC

	#pragma mark - WKScriptMessageHandler

	- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message {
    NSLog(@"%@",NSStringFromSelector(_cmd));
    NSLog(@"%@",message.body);

    if ([message.name isEqualToString:@"showMobile"]) {
        [self showMsg:@"JS调OC 我是下面的小红 手机号是:18870707070"];
    }
    
    if ([message.name isEqualToString:@"showName"]) {
        NSString *info = [NSString stringWithFormat:@"你好 %@, 很高兴见到你",message.body];
        [self showMsg:info];
    }
    
    if ([message.name isEqualToString:@"showSendMsg"]) {
        NSArray *array = message.body;
        NSString *info = [NSString stringWithFormat:@"这是我的手机号: %@, %@ !!",array.firstObject,array.lastObject];
        [self showMsg:info];
    	}
	}

其中, HTML 代码
	
<div>
<button class="btn" type="button" onclick="btnClick1()">小红手机号 "btnClick1()"</button>✨
  
<button class="btn" type="button" onclick="btnClick2()">📱小红"btnClick2()"</button>✨
        
<button class="btn" type="button" onclick="btnClick3()">📧给小红"btnClick3()"</button>
</div>
HTML中的 三个Button click 方法分分别对应着 相应方法并传入参数:
	
	function btnClick1() {
	window.webkit.messageHandlers.showMobile.postMessage(null)
	}

	function btnClick2() {  
	window.webkit.messageHandlers.showName.postMessage('小红')
	}

	function btnClick3() {
	window.webkit.messageHandlers.showSendMsg.postMessage(['13300001111', '我是小黄,周末喝酒去吧'])
	}
	
因为创建 WKWebView时已经加入了处理脚本,即 能够响应到 JS 的调用.

![](https://ws2.sinaimg.cn/large/006tNc79ly1fisbz3yzl3j30nk13wmzk.jpg)


另外也可以直接用 Safari 的开发工具来直接调试.Safari 打开开发菜单-选择正在运行的真机或模拟器-选择连接 就能进行调试了.这里我们测试:给小红发信息,改去唱歌
![](https://ws1.sinaimg.cn/large/006tNc79ly1fisc9fg7edj31j60zkk1k.jpg)
或者直接修改 HTML 代码,调用btnClick3() 也可以方便调试.

#### OC 调用 JS

	
	//网页加载完成之后调用JS代码才会执行，因为这个时候html页面已经注入到webView中并且可以响应到对应方法

	- (IBAction)btnClick:(UIButton *)sender {
    if (!self.wkWebView.loading) {
        if (sender.tag == 123) {
        // OC 请求 JS 給予回应,就得调用 JS 中已经存在的方法名,好得到相应
            [self.wkWebView evaluateJavaScript:@"alertMobile()" completionHandler:^(id _Nullable response, NSError * _Nullable error) {
                //TODO
                NSLog(@"%@ %@",response,error);
            }];
        }
        
        if (sender.tag == 234) {
            [self.wkWebView evaluateJavaScript:@"alertName('小黄')" completionHandler:nil];
        }
        
        if (sender.tag == 345) {
            [self.wkWebView evaluateJavaScript:@"alertSendMsg('18870707070','周末爬山真是件愉快的事情')" completionHandler:nil];
        }

    } else {
        NSLog(@"the view is currently loading content");
   	 	}
	}
	
其中 HTML 代码:

OC 传入参数要和 JS 中相对应以便接收	
	
	function alertMobile(){
		document.getElementById('mobile').innerHTML = 'OC调JS 我是小黄 手机号是:13300001111'
	}
	
	function alertName(msg){
		document.getElementById('name').innerHTML = '你好 ' + msg + ', 我也很高兴见到你'
	}
	
	function alertSendMsg(num,msg){
		document.getElementById('msg').innerHTML = '这是我的手机号:' + num + ',' + msg + '!!'
	
	}
	
这样我们 OC 执行相应方法时
![](https://ws2.sinaimg.cn/large/006tNc79ly1fisciy6yz0j30wu04674z.jpg)
JS 就能给出相应相应:

![](https://ws2.sinaimg.cn/large/006tNc79ly1fiscqc5oqjj30m012cgny.jpg)

