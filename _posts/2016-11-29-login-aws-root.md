---
layout: post
title: "在AWS EC 2上使用Root用户登录？"
date: 2016-11-29
author: "Alpaca"
subtitle: "远程连接"
catalog: true
categories: ios
tags:
   - AWS
      
---

1.确保在安全组中已经添加一条SSH 规则 端口22 任意IP

    端口：22


2.创建root的密码，输入如下命令：

    sudo passwd root


3.然后会提示你输入new password。输入一个你要设置的root的密码，需要你再输入一遍进行验证。

4.接下来，切换到root身份，输入如下命令：

    su root


5.使用root身份编辑亚马逊云主机的ssh登录方式，找到 PasswordAuthentication no，把no改成yes。输入：

    vim /etc/ssh/sshd_config


6.接下来，要重新启动下sshd，如下命令：

    sudo /sbin/service sshd restart


7.然后再切换到root身份

    su root


8.再为原来的”ec2-user”添加登录密码。如下命令：

    passwd ec2-user


按提示，两次输入密码。

9.修改sshd配置文件

    vi /etc/ssh/sshd_config


PermitRootLogin这行改为


    PermitRootLogin yes


PasswordAuthentication no改为

    PasswordAuthentication yes


UsePAM yes改为

    UsePAM no


10.重启AWS VPS，就可以使用root正常登陆了
