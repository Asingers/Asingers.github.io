---
layout: post
title: "iOS 修改默认屏幕截图设置"
date: 2017-03-12
author: "Asingers"
header-img: http://7xqmgj.com1.z0.glb.clouddn.com/2016-11-29-Wallions22023.jpeg
subtitle: "学习笔记"
catalog: true
categories: mac
tags:
   - mac
      
---


#### 修改截图名称
	
	defaults write com.apple.screencapture name "xxx"
	
#### 修改截图保存路径

	defaults write com.apple.screencapture location /path/
	
#### 修改截图文件格式

	defaults write com.apple.screencapture type jpg
	
#### 生效
 	
	killall SystemUIServer