---
layout: post
title: Ubuntu 搭建 Wordpress （LNMP）
subtitle: 基于 LNMP 环境
header-img: http://o6ledomfy.bkt.clouddn.com/20170812150253590862926.png
categories: ios
header-mask: 0.3
tags: 
    - AWS
    - Ubuntu
    - Wordpress

---

#### 环境搭建
使用[一键安装包](https://lnmp.org)，略过繁琐的过程，直接达成基本环境！ Get 😄

	wget -c http://soft.vpser.net/lnmp/lnmp1.4.tar.gz && tar zxf lnmp1.4.tar.gz && cd lnmp1.4 && ./install.sh lnmp

其中，我改掉了一些默认配置。过程只需要注意，输入Mysql的密码，其他基本默认就好。基本上半个小时就能跑完。

#### Mysql
然后就是Mysql相关操作了。

	# 登陆数据库
	mysql -u root -p
	
	# 创建用户
	CREATE USER 'wordpress-user'@'localhost' IDENTIFIED BY '123456';
	
	# 创建数据库
	CREATE DATABASE `wordpress-db`;
	
	#授予权限
	GRANT ALL PRIVILEGES ON `wordpress-db`.* TO "wordpress-user"@"localhost";
	
	# 刷新 MySQL 权限
	FLUSH PRIVILEGES;
	
	
#### Wordpress安装  
	
	# 下载，解压
	wget https://wordpress.org/latest.tar.gz
	tar -xzf latest.tar.gz
	
	# 将 wp-config-sample.php 文件复制到名为 wp-config.php 的文件。这样做会创建新的配置文件并将原先的示例配置文件原样保留作为备份。
	cp wp-config-sample.php wp-config.php
	
	# 配置数据库连接
	不再赘述，就是写对刚才我创建好的数据库名，用户名，密码
	
	# 移动Wordpress文件夹到html目录下并赋予权限
	chmod -R 755 ／xx/xx/wordpress
	chown -R www /xx/xx/wordpress
	
#### 安装
。。。

#### 小问题

遇到了一个设置固定连接就404的问题，解决办法如下：

	# 使用了LNMP一键包
	/usr/local/nginx/conf 中会有wordpress.conf
	
其中wordpress.conf 填写内容：
	
	location / {
	if (-f $request_filename/index.html){
                rewrite (.*) $1/index.html break;
        }
	if (-f $request_filename/index.php){
                rewrite (.*) $1/index.php;
        }
	if (!-f $request_filename){
                rewrite (.*) /index.php;
        }
	}
	
	# 注意，如果wordpress放在了子目录如abc.com/wordpress
	则其中 "location /" 则为 
	"location /wordpress/"，
	同理
	"rewrite (.*) $1/index.html break;" 
	则为"rewrite . /wordpress/index.html break;"
	后两个修改相似

然后在 `/usr/local/nginx/conf` 的nginx.conf 中添加
![](http://o6ledomfy.bkt.clouddn.com/20170812150253725396355.jpg)
然后 `lnmp restart` 一键重启就 OK 了
	


	
	
	
	


