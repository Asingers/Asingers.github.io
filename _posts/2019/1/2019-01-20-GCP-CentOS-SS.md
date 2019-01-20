---
layout: post
title: GCP CentOS SS Setup
subtitle: Google Cloud CentOS 搭建 Shadowshocks服务
categories: cloud
header-mask: 0.7
tags: 
    - Cloud
    - VPN

---

### 申请 Google VM instance

**步骤一：点击「TRY IT FREE」免费试用，使用 Google 帐号登录。**没有 Google 帐号就按流程一步步注册一个。
![](/images/post/20190120/gcpss1.png)

**步骤二：登录之后，点击「My Console」进入控制台。**Google Cloud 除了提供 VPS 服务以外还提供 Google Maps、Advertising APIs 等服务，可以在一个 Project 下统一管理，如果之前没建过 Project 的话，按系统要求创建一个即可。
![](/images/post/20190120/gcpss2.png)

**步骤三：按系统要求填下支付表单。**按表单要求填写即可，因为是免费试用，所以只验证信用卡信息，不扣费。注意国家可直接选成「China」，英文不会填的话，直接写拼音或者汉字，不会被系统鄙视的。
![](/images/post/20190120/gcpss3.png)

**步骤四：创建一个 VM instances，选择指定的配置。**其中，Zone（机房区域）一定要选择亚洲机房 asia-east1，深圳电信线路个人测试结果是访问 c 节点要稳定点。以下截图是最低配的主机类型，用来搭小飞机和个人站点完全够用。
![](/images/post/20190120/gcpss4.png)

### 安装「飞机」服务
**步骤一：打开在线终端。**
从管理页面找到刚创建好的 VM 主机，点击 SSH 按钮，在新窗口中打开在线终端。
![](/images/post/20190120/gcpss5.png)

**步骤二：运行安装命令。**
由于 Google VM 的权限控制比较严格，因此所有的命令前面都得加上 sudo ，Linode 等其他 VPS 提供商则不需要。
	
	sudo yum -y install python-setuptools
	sudo easy_install pip
	
以上命令依次安装了 easy_install、pip，它们都是 python 的模块包管理工具。参照下图 sudo pip install 命令，即可安装我们的「小飞机」：
![](/images/post/20190120/gcpss6.png)

**步骤三：使用以下命令启动小飞机服务。**
![](/images/post/20190120/gcpss7.png)

注意其中的 mgxqb 是密码,下面客户端连接时需要用到。当然也可以配置成其他端口、密码和加密类型。

**步骤四：最后一步，配置自启动配置文件。**
补充一下，据我观察 Google 的 VPS 会一个月重启一次，我们可以将小飞机服务添加到系统自启动脚本中，以便在服务器重启时自动开启服务。运行命令 sudo vim /etc/rc.local 编辑自启动配置文件。加入以下代码：
![](/images/post/20190120/gcpss8.png)

**步骤五：配置防火墙，允许外部 IP 访问 8388 端口。**
如果是 Linode 等其他 VPS 服务商，可直接运行以下命令修改，但 Google VM 这样配置不会生效。

	iptables -A INPUT -p tcp  --dport 8388 -j ACCEPT
	/etc/init.d/iptables save
	/etc/init.d/iptables restart
	
因为 Google VM 的防火墙配置统一由在线后台完成。如下图，找到对应 VPS 的「Network -> default」：
![](/images/post/20190120/gcpss9.png)

添加一条防火墙规则，允许外部所有来源 IP（Allow from any source），使用本地 tcp:8388 端口。
![](/images/post/20190120/gcpss10.png)

**步骤六：最后配置静态 IP。**
由于 Google VM 默认使用动态 IP，服务器重启后 IP 可能会发生改变。因此需要参照下图，将该台 VM 配置成静态 IP：
![](/images/post/20190120/gcpss11.png)

### 4、安装「小飞机」客户端
Mac 用户请[点击此处](https://github.com/shadowsocks/shadowsocks-iOS/releases)下载 ，下载安装后，进行配置，很简单不再赘述，填入上边设置的IP，端口，加密方式即可。

Windows 用户[点击此处](https://github.com/shadowsocks/shadowsocks-windows/releases) 下载

手机下载相应的客户端进行配置即可。




