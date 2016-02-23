---
layout: post
title: "CocoaPods报错：The dependency `xxx` is not used in any concrete target"
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
	The dependency `` is not used in any concrete target
	The dependency `AFNetworking ` is not used in any concrete target

如果不巧你看到CocoaPods的升级Beta版测试并进行了升级,

	sudo gem install cocoapods --pre
	
那么你就有可能遇到上面那个问题.

### 解决方法

Podfile文件请按照如下格式:

	platform :ios, '8.0'
	target 'MyApp' do
	pod 'AFNetworking', '~> 2.6'
	pod 'ORStackView', '~> 3.0'
	pod 'SwiftyJSON', '~> 2.3'
	end
	
其中 Myapp就是你的工程的Target名

至此,问题解决