---
layout: post
title: iOS Universal Links Setup
subtitle: 配置通用链接
categories: Mac
header-mask: 0.7
tags: 
    - iOS
    - Xcode

---

### 服务器端配置
新建一个没有后缀名，且名为apple-app-site-association的文件，并写入
	
	可以有多个app配置，只需要一一对应互不影响即可
	{
	"applinks": {
        "apps": [],
        "details": [
            {
                "appID": "teamID.bundleId”,
                "paths": ["/sample","*"]
            },
            {
                "appID": "example.com.apple.example",
                "paths": [ "*" ]
            	}
        	]
    	}
	}


其中 appID 的 格式为 teamID.bundleId.
path为URL路径，即可以响应的URL

* 使用*配置，则整个网站都可以使用

* 使用特定的URL，例如/sample来指定某一个特殊的链接

* 在特定URL后面添加*，例如 /videos/sample/2018/*, 来指定网站的某一部分

* 除了使用*来匹配任意字符，你也可以使用 ?来匹配单个字符，你可以在路径当中结合这两个字符使用，例如 /video/*/sample/201?/

配置好之后，文档丢到根目录下或者.well-known路径下。应保证https://yourdormain.com/apple-app-site-association 可以正常访问

### Xcode配置

确保bundleID 一致，在项目的 Capablities 中开启 Associated domains 并添加 applinks:yourdormain

此时在safari中输入yourmain 可以跳转到app,系统代理方法⬇️

	-(BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray * 	_Nullable))restorationHandler{
	 NSLog(@"userActivity : %@",userActivity.webpageURL.description);
	    return YES;
	}
	能够拿到universal links 链接
	

