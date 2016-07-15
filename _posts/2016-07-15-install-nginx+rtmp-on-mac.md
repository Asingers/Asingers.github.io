---
layout: post
title: "Mac搭建nginx+rtmp服务器"
date: 2016-07-15
author: "Asingers"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
   - Mac
   - 服务器
   
---

#### 一、安装Homebrow

已经安装了brow的可以直接跳过这一步。
执行命令

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


如果已经安装过，而想要卸载：

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"


#### 二、安装nginx

先glone nginx项目到本地：

    brew tap homebrew/nginx


执行安装：

    brew install nginx-full --with-rtmp-module


安装过程比较缓慢，耐心等待
通过操作以上步骤nginx和rtmp模块就安装好了，下面开始来配置nginx的rtmp模块

首先来看看我们的nginx安装在哪里了

    brew info nginx-full


执行上面的命令后我们可以看到信息

nginx安装所在位置

    /usr/local/Cellar/nginx-full/1.10.1/bin/nginx


nginx配置文件所在位置

    /usr/local/etc/nginx/nginx.conf


#### 三、运行nginx

执行命令 ，测试下是否能成功启动nginx服务

    nginx


命令行如下图所示

在浏览器地址栏输入：[http://localhost:8080](http://localhost:8080)（直接点击）
如果出现

Welcome to nginx!.03


代表nginx安装成功了

如果终端上提示

    nginx: [emerg] bind() to 0.0.0.0:8080 failed (48: Address already in use)


则表示8080
端口被占用了, 查看端口PID

    lsof -i tcp:8080


kill掉占用8080端口的PID

    kill 9603（这里替换成占用8080端口的PID）


然后重新执行nginx

nginx常用方法：重新加载配置文件

    nginx -s reload


重新加载日志:

    nginx -s reopen


// 停止 nginx

    nginx -s stop


// 有序退出 nginx

    nginx -s quit


#### 四、配置rtmp

现在我们来修改nginx.conf这个配置文件，配置rtmp
复制nginx配置文件所在位置

    /usr/local/etc/nginx/nginx.conf


打开Finder Shift + command + G前往，用记事本工具打开nginx.conf

    http {
        ……
    }


在http节点后面加上rtmp配置：

    rtmp {
    
      server {
          listen 1935;
    
    
        #直播流配置
          application rtmplive {
              live on;
          #为 rtmp 引擎设置最大连接数。默认为 off
          max_connections 1024;
    
    
           }
    
    
          application hls{
    
              live on;
              hls on;
              hls_path /usr/local/var/www/hls;
              hls_fragment 1s;
          }
       }
    }


#### 六、安装ffmepg工具

    brew install ffmpeg


安装这个需要等一段时间等待吧 然后准备一个视频文件作为来推流，然后我们在安装一个支持rtmp协议的视频播放器，Mac下可以用VLC


ffmpeg安装完毕0.5


ffmepg 安装完成后可以开始推流了
