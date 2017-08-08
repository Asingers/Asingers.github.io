---
layout: post
title: Mac 升级 Ruby 版本
subtitle: 😫 ➡️ 😄
categories: ios
header-mask: 0.3
tags: 
    - Mac

---

> 有时候最让你抓狂的就是无意义的等待  
	
最近Cocoapods对Ruby版本要求变高了，需要升级自带的Ruby版本。本来以为就是几个命令的事，没想到因为网速的问题翻了车。因为我开着VPN，想着总能解决吧。。。

	1. curl -L get.rvm.io | bash -s stable  

环境变量：

	source ~/.bashrc  
	source ~/.bash_profile  
	
测试是否安装正常：

	rvm -v 
	
用RVM升级Ruby：
	
	#列出已知的ruby版本  
	rvm list known  
	# 安装对应版本
	rvm install xxx
	
讲道理应该是这样无痛的过程，但是 `rvm install` 太慢了，试着更换Ruby源：

	# 列出现在的
	gem sources -l
	
	# 移除现有的
	gem sources -r xxx
	
	# 添加新的
	gem source -a https://gems.ruby-china.org
	
这样速度总该上去了吧。
7M的东西要等4个小时？
![](http://o6ledomfy.bkt.clouddn.com/20170808150219689164312.jpg) 

怎么能忍，找到了解决办法：

	echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db
	
全程无痛。

