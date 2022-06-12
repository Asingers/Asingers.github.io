---
layout: post
title: Run Openwrt on Raspberry Pi 4B+ with samba server
description: 利用树莓派做旁路由并顺手开启samba 服务
categories: RaspberryPi
header-mask: 0.7
tags: RaspberryPi

---

### 固件安装

从这里下载[固件下载地址](https://openwrt.cc/releases/targets/bcm27xx/bcm2711/) 对应的固件，将固件写入U 盘。[写盘工具Etcher 下载地址](https://github.com/balena-io/etcher)首次安装下载- factory 的后续可以下载-upgrade 的进行升级。我选了squashfs 格式的，试了几次树莓派没启动起来，不知道什么鬼，然后由重新写入了ext4 格式的了。

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-26-14.jpg)

启动树莓派，稍后连接名为Openwrt 的Wi-Fi，并登陆默认管理页面192.168.1.1。用户名*root*,密码*password* 此时先不要将网线连接树莓派，因为上级路由器的管理页面有可能就是192.168.1.1

### 修改接口

1. 在网络-接口设置中￼修改 Lan 接口为静态IP地址，例如192.168.0.102，网段和上级路由器保持一致，例如你的上级路由器管理地址是192.168.0.1
2. 设置IP 网关为上级路由器网关192.168.0.1
3. 子网掩码255.255.255.0
4. IPv4 广播 192.168.0.255

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-07-07.jpg)

将树莓派有线连接到路由器，就可以用上边的静态IP 192.168.0.102 登录管理页面了

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-13-36.jpg)

### 开启DHCP
因为要将树莓派作为旁路有网关，IP 地址由openwrt 分配，开启的同时记得把上级路由器的DHCP 关掉。

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-15-36.jpg)

物理设置中取消桥接，选择Lan 接口 eth0

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-16-13.jpg)

防火墙中把SYN-flood 防御关掉。

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-16-46.jpg)

可以关闭树莓派的Wi-Fi 接口，仍然使用路由器的无线信号，毕竟信号会强一点，树莓派只作为网关转发，不做信号发射。连接路由器发出的Wi-Fi信号，在详细信息中确认一下网关是否为 192.168.0.102 也就是树莓派的IP地址。设置就基本完成了。
### SSR

导入服务器站点之后开启服务，整个网路都可以🪜了

### SAMBA

如果有NAS 的需求，可以把硬盘连接到树莓派，在挂在点中添加硬盘。挂在成功后可以看到硬盘的挂载路径。

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-19-46.jpg)

在openwrt 的网络共享中开启服务，添加一个共享配置，目录就是硬盘的挂载路径，例如/mnt/sda1 ，勾选可浏览，新文件和目录权限掩码均为0777，允许用户为root

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-18-16.jpg)

在网络共享的编辑模版菜单中，将invalid user 这一行注释掉

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-35-54.jpg)

我们通过终端修改一下samba root 用户的密码`smbpasswd -a root`

输入新的密码，然后重启服务 `/etc/init.d/samba restart`

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-21-44.jpg)

然后可以通过连 `smb://192.168.0.102` 登录到NAS 了

![网络配置](https://9dic.com/images/post/2022/Xnip2022-06-12_22-23-26.jpg)