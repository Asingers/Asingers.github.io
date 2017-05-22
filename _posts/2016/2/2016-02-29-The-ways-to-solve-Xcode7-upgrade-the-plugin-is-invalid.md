---
layout: post
title: "升级Xcode7插件无效的解决方法"
subtitle: "The ways to solve Xcode7 upgrade the plugin is invalid"
date: 2016-02-29 
author: "Alpaca"
categories: ios
tags:
    - iOS
    - Dev
---
以VVDocumenter-Xcode为例:


从Xcode 5开始，苹果要求加入UUID证书从而保证插件的稳定性。因此Xcode版本更新之后需要在VVDocumenter-Xcode的Info.plist文件中添加Xcode的UUID。

## 步骤如下：

### 一、查看Xcode的UUID

#### 方式1

在终端执行

    defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID

<img src="http://images.90159.com/11/vvdocumenter3.jpg" alt="" class="shadow"/>

拷贝选中的字符串。

#### 方式2

在/Applications目录中找到Xcode.app，右键”显示包内容”，进入Contents文件夹，双击Info.plist打开，找到DVTPlugInCompatibilityUUID，拷贝后面的字符串。

### 二、添加Xcode的UUID到VVDocumenter-Xcode的Info.plist文件

#### 方式1 插件已经安装完成

1、打开xcode插件所在的目录：~/Library/Application Support/Developer/Shared/Xcode/Plug-ins；

2、选择已经安装的插件例如VVDocumenter-Xcode，右键”显示包内容”；

3、找到info.plist 文件，找到DVTPlugInCompatibilityUUIDs的项目，添加一个Item，Value的值为之前Xcode的UUID，保存。

![VVDocumenter1](http://images.90159.com/11/vvdocumenter4.jpg)

#### 方式2 插件还未安装/重新安装

1、从GitHub克隆仓库到本地，在Xcode中打开项目，选择项目名称，在TAGETS下选中VVDocumenter-Xcode；

2、选择Info，找到DVTPlugInCompatibilityUUIDs的项目，添加一个Item，Value的值为之前Xcode的UUID；

3、Build项目，VVDocumenter-Xcode会自动安装。


### 三、重启Xcode

Xcode 6之后，重启Xcode时会提示“Load bundle”、 “Skip Bundle”，这里必须选择“Load bundle”，不然插件无法使用。
你也可以尝试下边的命令来修复插件

	curl -s https://raw.githubusercontent.com/ForkPanda/RescueXcodePlug-ins/master/RescueXcode.sh | sh  


	  
	
