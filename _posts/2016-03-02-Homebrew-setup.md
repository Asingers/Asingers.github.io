---
layout: post
title: "Homebrew安装时”-bash:brew:command not found”的问题"
subtitle: "Homebrew安装"
date: 2016-03-02
author: "Asingers"
categories: ios
tags:
    - iOS
    - Dev
    - Linux
    - Brew
---

首先，Homebrew安装。大家应该大多是按照[Homebrew]官网上的安装教程安装的，简单明了。但是在终端中安装完成之后，大家运行brew时，却发现终端提示”-bash:brew:command not found”，无法找到brew命令。

网上查了半天也没有找到合适的解决办法，最后只好无奈重新卸载掉Homebrew重装。
**这时，问题的关键来了:**

在重装完成时，终端提示

**/usr/local/bin不在PATH中**

所以，又去各种折腾一番去弄什么/.bash_profile啊什么的，墨迹半天也没有设置好。最后，怀着试一试的想法，找到了最终解决方案：

1.打开终端获取管理员权限
2.跳转至/etc下修改profile文件，关于什么是profile文件，可以去[这里]。
3.用vim打开，在profile文件最后添加如下语句：
PATH=“.;$PATH:/usr/local/bin”
注意，若以前修改过该文件，或者PATH中已有多个路径，只需在后面添加即可，使用：隔开。
4.好了，保存退出后，再次键入brew命令，有惊喜发生！

**卸载Brew ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"**  
***
**安装HomeBrew /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"**  
**也许会遇到权限问题 需要修改/urs/local权限**
HomeBrew主页: [Home](http://brew.sh/)


