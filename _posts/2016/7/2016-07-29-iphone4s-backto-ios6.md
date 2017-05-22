---
layout: post
title: "iPhone 4S 降级iOS 6"
date: 2016-07-29
author: "Alpaca"
subtitle: "Tips"
catalog: true
categories: life
tags:
   - Life
   - iOS
---

第一部分：准备工作

[首先点击：下载好3个文件](http://pan.baidu.com/s/1dDxuYxr?errno=0&errmsg=Auth%20Login%20Sucess&stoken=fe1c68f066364d18bcb0f70352cc45390b2c7224ea2c9c6ab7e9d3136d1fd0070987ff5fdf2f71f5d8d77c394e6cee52902640e759d8e36aea0f85950141945b2d4869fc40df&bduss=668a1e6202648eb1a12b3d9d704809452e4bb452ae4ee483a1754028d83f4086cb74c1574280bab47c0c0e59992eed5849ce1c8c8123bb688deb7e2ec01c7c19683807135176e9599bd02d70a22abea0fc71a77ea710d3ab6afe2247faef408d61df938ba0482b6b4920d45738dec93cdf5b2a4344b004059ede4cb1d46f5666d446007e83417d686789e962064eb1bd4cc436885bbd9dbf64e5e072f54c166a3b88a943364ab66c4d0c9ab3f229d65d3b93898c813cb382350c4d4c5705a6af0bad76dec8f0&ssnerror=0)

1：下载工具包odysseusOTA4WIN.rar，解压在任意位置，然后打开未命名文件夹3，将此文件夹下的fistmedaddy.ipsw复制到idevicerestore for Windows文件夹内，并将idevicerestore for Windows文件夹整个复制到C盘根目录下。

2：安装Win32OpenSSL-1_0_2c.exe

3：安装winscp5.7.3.5438setup.1432114150.exe  安装最新版的itunes

4：手机必须越狱，越狱后打开Cydia，添加源apt.178.com，再搜索安装插件openSSH和Core Utilities

>**第二部分：配置并登陆**



1、打开wifi，点击iphone的无线局域网，点击蓝色的i图标，记住第一个ip地址.




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435905298784018927.png)




2、打开winSCP，主机地址填ip地址.用户名root，密码alpine，文件协议SCP，点击登录.




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435906846689099054.jpg)




3、找到你odysseusOTA4WIN.rar里的kloader与pwnediBSS两个文件，选中后，拖到右边的窗口里




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435908748733014589.jpg)




4、在winSCP里按ctrl+T打开终端，输入chmod +x kloader，点击执行




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435908778089065508.jpg)




5、输入./kloader pwnediBSS，点击执行，执行之后iphone黑屏，弹出错误提示，是因为进入了DFU模式，手机连接中断导致的，不用管直接点确定关闭WinSCP即可




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435907879190083062.jpg)




**第三部分：降级开始**




1、将手机用数据线连接电脑.win+R呼出运行框，输入cmd打开命令提示符.




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435908078353062872.png)




3、输入“cd\”，然后回车.




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435908110939067756.png)




4、输入“cd idevicerestore for Windows”，然后回车.




5、输入“idevicerestore.exe空格-e空格fistmedaddy.ipsw”（注意空格），然后回车.之后会执行命令，看着就行了。




![苹果iPhone4S怎么降级到iOS6.1.3图文教程](http://d.image.i4.cn/i4web/image//upload/20150703/1435908671023046884.png)




6、等待命令行走完，就OK了.
