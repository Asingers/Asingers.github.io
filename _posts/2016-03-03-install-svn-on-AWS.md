---
layout: post
title: "在AWS上搭建SVN"
subtitle: "How To Install SVN On AWS Liunx"
date: 2016-03-03
author: "Asingers"
categories: ios
tags:
    - Linux
    - AWS
    - SVN
---
## AWS 安装SVN
一句话安装

	yum install subversion
	
后续需要开启Http支持所以就直接把相关包安装了

	mod_dav_svn
	mod_perl
	#(用于支持WEB方式管理SVN服务器)	
## Yum已经安装的包冲突?
因为我之前已经配好了很多环境 所以一些包已经安装 所以就会产生冲突 只需要卸载其中一个包就可以了  
比如我这里卸载了:
<hr>
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/in_post_imgsvn.png" alt="" class="shadow"/>
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/in_post_imgsvn1.png" alt="" class="shadow"/>

***
## 创建目录:

	mkdir  -p  /home/svnroot/svndata/repos1  
  
加上参数P，是如果没有父目录则自动创建  
注意： /home/svnroot/svndata在这里将是所有仓库的根目录，repos1是其中的一个仓库。 

## 创建仓库:

	svnadmin create  /home/svnroot/svndata/repos1  
  
这里使用SVN将repos1建立为仓库。则在repos1文件夹里会生成一系列对于repos1仓库相应的配置文件  

## 配置仓库:
进入/svndata/repos1/conf，会发现有几个配置文件 , 修改svnserve.conf

	vi svnserve.conf  
  
	#打开这个配置文件，可以看到很多配置项已经注释掉了，只需要按下面这几项修改就可以了  
  
	[general]  
  
	anon-access = none  
  
	auth-access = write  
  
	password-db = passwd  
  
	authz-db=authz  
	
## 目录控制文件authz （or叫权限控制文件）:
可以更改其中admin的名字
vi authz  
  
默认是没有配置的，要参照下面示例来配置  
  
	[groups]  
	admin = svnadmin  
	[repos1:/]  
	@admin = rw  
	svnadmin = rw  
  
	#上面的配置权限控制文件的配置格式如下：  
  
	[groups]  
	<用户组名> = <用户1>,<用户2>, ……  
	[<版本库>:/项目/目录]  
	@<用户组名> = <权限> 
	<用户名> = <权限>  
	#其中，方框号内部分可以有多种写法:  
  
	/，表示根目录及以下。根目录是svnserve启动时指定的，我们指定为/home/svnadmin/svndata。这样，/就是表示对全部版本库设置权限。  
  
	repos1:/，表示对版本库1设置权限  
	repos2:/occi，表示对版本库2中的occi项目设置权限  
	repos2:/occi/aaa,，表示对版本库2中的occi项目的aaa目录设置权限  
  
权限主体可以是用户组、用户或*，用户组在前面加@，*表示全部用户。权限可以是w、r、wr和空，空表示没有任何权限。 

## 修改用户密码文件passwd:

	vi passwd  
  
	#默认也是没有配置任何用户的，可按下面配置示例配置  
  
	[users]  
  
	svnadmin = 123456  
	# 之后可以进行更改
  
	#用户密码的配置格式：  
  
	[users]  
  
	<用户1> = <密码1>  
  
	<用户2> = <密码2>  
  
	注意：这里的配置文件，除了注释外每行都必须顶行，否则又会报错了。  

## 启动SVN:

	svnserve -d -r /home/svnadmin/svndata  
  
	-d表示在后台运行，-r表示……  
  
	#注意：这里是/home/svnadmin/svndata，并非/home/svnadmin/svndata/repos1。这是SVN使所有仓库根目录都生效的命令，并非某个仓库。这里必须注意。 
	
## 基本测试

	svn co svn://yourhost/repos1  
  
	checkout的时候，会要求输入用户名密码，只有配置了的用户才能验证通过  
	
## 开启HTTP支持
接下来我要进行相关配置,以达到可以通过http://hostname/repos1可以访问的目的
确保相关包已经安装,如果你之前什么环境都没打的话,

	一句话安装:
	yum install subversion mysql-server httpd mod_dav_svn mod_perl sendmail wget gcc-c++ make unzip perl* ntsysv vim-enhanced
说明：

	subversion (SVN服务器)
	mysql-server (用于codestriker)
	httpd 
	mod_dav_svn    mod_perl (用于支持WEB方式管理SVN服务器)
	sendmail (用于配置用户提交代码后发邮件提醒)
	wget gcc-c++ make unzip perl* (必备软件包)
	ntsysv vim-enhanced (可选)
<hr>
由于SVN服务器的密码是明文的，HTTP服务器不与支持，所以需要转换成HTTP支持的格式。我写了一个Perl脚本完成这个工作.

	# cd /home/svnroot/svndata/repos1/conf
	# vim PtoWP.pl
	
内容:

	#!/usr/bin/perl
	# write by Asingers, 2016-03-03

	use warnings;
	use strict;

	#open the svn passwd file
	open (FILE, "passwd") or die ("Cannot open the passwd file!!!\n");

	#clear the apache passwd file
	open (OUT_FILE, ">webpasswd") or die ("Cannot open the webpasswd file!!!\n");
	close (OUT_FILE);

	#begin
	foreach (<FILE>) {
	if($_ =~ m/^[^#].*=/) {
	$_ =~ s/=//;
	`htpasswd -b webpasswd $_`;
	}
	}
执行:

	# chmod +x PtoWP.pl
	# ./PtoWP.pl
现在目录下会多一个webpasswd文件。
<hr>
## 修改httpd.conf，添加关于SVN服务器的内容
编辑/etc/httpd/conf/httpd.conf，在最后添加如下信息:
	
	# SVN 
	<Location /repos1>
	DAV svn
	SVNPath /home/svnroot/svndata/repos1/
	AuthType Basic
	AuthName "svn for repos1"
	AuthUserFile /home/svnroot/svndata/repos1/conf/webpasswd
	AuthzSVNAccessFile /home/svnroot/svndata/repos1/conf/authz
	Satisfy all
	Require valid-user
	</Location>

	修改svn目录的属主为apache帐号：chown -R apache.apache /home/svnroot/svndata/repos1/

## 重启Web服务器：

	sudo service httpd restart
	
Done!

