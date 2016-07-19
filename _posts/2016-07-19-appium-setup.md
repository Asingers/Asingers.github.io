---
layout: post
title: "Appium iOS 自动化测试环境搭建 "
date: 2016-07-19
author: "Asingers"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
   - Mac
   - 测试
   
---

#### 安装Brew
已安装请忽略  

	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  

#### 安装Node
已安装请忽略  

	brew install node  


#### 安装appium-doctor  
appium-doctor是一个用于验证appium安装环境的工具，可以诊断出Node/iOS/Android环境配置方面的常见问题。  

	npm install appium-doctor -g  
	
安装完毕后，执行appium-doctor命令即可对Appium的环境依赖情况进行检测；指定--ios时只针对iOS环境配置进行检测，指定--android参数时只针对Android环境配置进行检测，若不指定则同时对iOS和Android环境进行检测。  

	$ appium-doctor --ios                                                                                                                               
	info AppiumDoctor ### Diagnostic starting ###
	info AppiumDoctor  ✔ Xcode is installed at: /Applications/Xcode.app/Contents/Developer
	info AppiumDoctor  ✔ Xcode Command Line Tools are installed.
	info AppiumDoctor  ✔ DevToolsSecurity is enabled.
	info AppiumDoctor  ✔ The Authorization DB is set up properly.
	info AppiumDoctor  ✔ The Node.js binary was found at: /usr/local/bin/node
	info AppiumDoctor  ✔ HOME is set to: /Users/Leo
	info AppiumDoctor ### Diagnostic completed, no fix needed. ###
	info AppiumDoctor
	info AppiumDoctor Everything looks good, bye!
	info AppiumDoctor
	
#### 安装Appium.app  

[官网](http://appium.io/)下载安装  

#### 上手调试  
#### 安装ideviceinstaller备用

	brew install ideviceinstaller  
	
#### 配置
针对模拟器而言  
1. x.app 路径  
2. 相对应的设备名  


如图:
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-19_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-19%20%E4%B8%8A%E5%8D%8811.04.16.png" alt="" class="shadow"/>  

针对真机而言:  
1. 运行demo到设备  
2. 打开设置-开发者-Enable Automation  
3. BundleID
4. 相对应设备名  
5. UUID  

如图:
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-19_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-19%20%E4%B8%8A%E5%8D%8811.10.23.png" alt="" class="shadow"/>

#### 启动 


