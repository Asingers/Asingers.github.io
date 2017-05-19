---
layout: post
title: "Mac iOS Jenkins 集成"
date: 2016-07-28
author: "Alpaca"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-25_2016objc.jpeg"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - Mac
   - iOS
   - 开发
   - Jenkins
   
---

#### 安装

通过pkg 或者 brew安装都可以,不再赘述,之前提到过,今天主要整理一下具体配置. 选择推荐插件完成安装即可 v2.15
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-28_10:30:53.jpg" alt="" class="shadow"/>  

#### 插件安装
1.Keychains and Provisioning Profiles Management  

2.Xcode-plugin  

3.subversion  

按需安装,已安装请忽略.  

#### 配置

上传login.keychain  
填写Code Sign  
找不到的可以通过cd 路径 把文件拷贝出来 并修改777权限    

eg :sudo cp login.keychain '/Users/xxx/Desktop/key'  
    sudo chmod 777 login.key    
    
上传描述文件  
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-28_10:51:18.jpg" alt="" class="shadow"/>  

配置源码,SVN为例  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-28_10:52:30.jpg" alt="" class="shadow"/>  

配置构建环境  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-28_10:54:11.jpg" alt="" class="shadow"/>  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-28_10:56:38.jpg" alt="" class="shadow"/>  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-28_10:59:36.jpg" alt="" class="shadow"/>  

