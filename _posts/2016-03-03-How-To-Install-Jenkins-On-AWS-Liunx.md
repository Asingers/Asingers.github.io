---
layout: post
title: "在AWS上安装Jenkins"
subtitle: "How To Install Jenkins On AWS Liunx"
date: 2016-03-03
author: "Asingers"
categories: ios
tags:
    - iOS
    - Linux
    - AWS
    - Jenkins
---

这是我的实际应用,应该是通用于Linux的.
我首先按照我的博客里的两篇教程进行环境搭建.这个过程可能已经很友好的安装了一些东西.所以接下来的过程很顺畅.

## 下载Jenkins:

	sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
	sudo rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
	
## 安装

	yum install jenkins
	
**Jenkins 运行需要Java环境 如果已经安装就略过**

## 安装Java

	sudo yum install java  

## 启动/停止/重启

	sudo service jenkins start/stop/restart
	sudo chkconfig jenkins on

> 这是我参考的官方文档 [官方文档](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Red+Hat+distributions)  



	


 