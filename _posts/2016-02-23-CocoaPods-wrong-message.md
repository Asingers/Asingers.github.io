---
layout: post
title: "The dependency "xxx" is not used in any concrete target"
subtitle: "解决方案"
date: 2016-02-23 
author: "Asingers"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/header_imgnsarry.jpg"
categories: ios
tags:
    - iOS
    - Dev
---
### 问题

	The dependency "xxx" is not used in any concrete target
	The dependency "AFNetworking" is not used in any concrete target

如果不巧你看到CocoaPods的升级Beta版测试并进行了升级,

	sudo gem install cocoapods --pre
	
那么你就有可能遇到上面那个问题.

### 解决方法

Podfile文件请按照如下格式:

	platform :ios, '8.0'
	target 'MyApp' do
	pod 'xxx', '~> 2.6'
	pod 'xxx', '~> 3.0'
	pod 'xxx', '~> 2.3'
	end
	
其中 Myapp就是你的工程的Target名

至此,问题解决
<hr>

当然你也可以通过安装指定版本来解决:

移除pod组件
这条指令会告诉你Cocoapods组件装在哪里 :

	which pod
	
你可以手动移除这个组件 :

	sudo rm -rf <path>
	
移除 RubyGems 中的 Cocoapods程序包

查看gems中本地程序包

	gem list
	
eg:

	*** LOCAL GEMS ***
	activesupport (4.1.8, 3.2.21)
	bigdecimal (1.2.0)
	CFPropertyList (2.2.8)
	claide (0.7.0)
	cocoapods (0.35.0, 0.34.1, 0.34.0)
	cocoapods-core (0.35.0, 0.34.1, 0.34.0)
	cocoapods-downloader (0.8.0, 0.7.2)
	cocoapods-plugins (0.3.2)
	cocoapods-trunk (0.4.1, 0.2.0)
	cocoapods-try (0.4.2)
	colored (1.2)
	escape (0.0.4)
	fuzzy_match (2.0.4)
	i18n (0.6.11)
	io-console (0.4.2)

找到Cocoapods的程序包

	cocoapods (0.35.0, 0.34.1, 0.34.0)
	cocoapods-core (0.35.0, 0.34.1, 0.34.0)
	cocoapods-downloader (0.8.0, 0.7.2)
	cocoapods-plugins (0.3.2)
	cocoapods-trunk (0.4.1, 0.2.0)
	cocoapods-try (0.4.2)

移除程序包

	sudo gem uninstall cocoapods -v 0.35.0
	
```	
Successfully uninstalled cocoapods-0.35.0```

然后安装指定版本的Cocoapods

	sudo gem install cocoapods -v 0.34.4

