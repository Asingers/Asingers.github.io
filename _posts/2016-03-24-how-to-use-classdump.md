---
layout: post
title: "如何使用 Class-Dump 获取 .h 文件"
date: 2016-03-24
author: "Asingers"
subtitle: "iPA 撬壳"
tags:
     - iOS
     - Mac
---

> 使用Class-Dump可以获取iPA所有.h文件,对于学习来说这已经足够,闲话我们以后再絮.


# 安装
官网下载[Class-Dump](http://stevenygard.com/),安装dmg,将安装后的class-dump 放到/usr/local/bin 目录下.这样我们就可以在终端使用class-dump命令了.

# 使用
命令使用:

	class-dump -H 源路径 -o 目的路径   
	
这个命令简单明了.
下边是个例子:  

	class-dump -H /Users/zhangyuanjie/Desktop/Fire/daily.app -o /Users/zhangyuanjie/Desktop/Fire/heads

回车,马上我们就能看到在文件夹中已经有了所有的.h文件. 
 
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/post_imghh.png" alt="" class="shadow"/>

  
`注意:`你应该已经注意到,源路径是.app所在路径,(这个是某乎出品的日报)在我们未砸壳之前,(砸壳之后在研究)简单的方法是去第三方软件管理软件下载越狱设备用iPA文件,解压缩,显示包内容,balalala...  

目前,到此为止,看到关于iOS安全的专题文章产生了很大兴趣,之后更多内容,等待更新,这个过程貌似很有乐趣,我先看看.h中的方法去.
