---
layout: post
title: Install Gitlab EE on Raspberry Pi 4B+
description: 在树莓派上搭建 Gitlab 服务
subtitle: 在树莓派上搭建 Gitlab 服务
categories: RaspberryPi
header-mask: 0.7
tags: Xcode

---

首先更新一下系统
```
sudo apt-get update 
```

安装必要依赖
```
sudo apt-get install curl openssh-server ca-certificates postfix apt-transport-https
```

下载安装包
```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash 
```

编辑配置文件 **vi /etc/gitlab/gitlab.rb**，修改 **EXTERNAL_URL** 字段，设置访问地址。

启动服务
```
sudo gitlab-ctl reconfigure
```

