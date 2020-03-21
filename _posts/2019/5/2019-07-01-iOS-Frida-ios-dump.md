---
layout: post
title: Frida-ios-dump
description: iOS App Crack
categories: iOS
header-mask: 0.7
tags: Crack

---
## Frida-iOS-Dump
iOS App 砸壳，什么是壳，对于iOS 安全性相关的内容不在这里说，有兴趣的可以看看官方文档[iOS 安全保护](https://images.apple.com/cn/business/resources/docs/iOS_Security_Guide.pdf)，现在要做的就是去除安全验证。获得一个可以进行Debug 的 iPA 文件。
### Mac 安装 frida
确保Python 环境已经安装
`sudo pip install frida`

可能会遇到权限问题，比如 

```Operation not permitted: '/tmp/pip-uW0fNP-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/six-1.4.1-py2.7.egg-info'
```

可以通过尝试`sudo pip install frida --ignore-installed six` 再次安装。
对于权限的问题，如果你经常折腾，可以关闭Mac 的 SIP。

### 手机安装 frida

没什么好说的，Cydia 添加源 `https://build.frida.re`，搜索frida 然后安装。

安装好之后，在Mac 终端通过命令 `frida-ps -U` 可以看到手机上的App ，如果OK 说明准备工作就绪。

### SSH 连接

用终端登录到手机，同样需要在 Cydia 中安装插件 OpenSSH，然后通过`ssh root@ip` 登录，默认密码是`alpine`

### USB 连接

brew 安装以下两个包，`brew install libimobiledevice`，`brew install usbmuxd`，安装后通过 `iproxy 2222 22` 打开端口映射。然后终端会提示waiting for connection . 这时候再新开一个终端 `ssh -p 2222 root@localhost` 就可以登录到手机了。到这里所有环境就绪。

### 🔨

接下来需要下载[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)，然后 cd 到文件夹下，运行 ` ./dump.py appname` 或者 ` ./dump.py bundleid` 即可，成功的话砸壳后的iPA  就会出现在当前文件夹内。
该命令会自动打开App，确保手机没有锁屏，如果提示进行挂起失败，可以尝试手动先打开App，然后再执行命令。


