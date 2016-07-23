---
layout: post
title: "Mac搭建nginx+rtmp服务器 推流"
date: 2016-07-15
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/2016live.jpeg"
author: "Asingers"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
   - Mac
   - 服务器
   
---

### 环境

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

加上rtmp配置：

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
    
    
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%2023.39.47.png" alt="" class="shadow"/>


#### 五、安装ffmepg工具

    brew install ffmpeg


安装这个需要等一段时间等待吧 然后准备一个视频文件作为来推流，然后我们在安装一个支持rtmp协议的视频播放器，Mac下可以用VLC

ffmepg 安装完成后可以开始推流了  

### 推流

#### 1.推流MP4文件

- 视频文件地址：/Users/xu/Desktop/bangbangbang.mp4
- 推流拉流地址：rtmp://localhost:1935/rtmplive/home
- acc：RTMP的音频格式
- flv： RTMP的视频格式  

	`ffmpeg -re -i /Volumes/娱乐/Movie/ximalaya.mp4 -vcodec libx264 -acodec aac -f flv rtmp://localhost:1935/rtmplive/home`

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%2023.38.58.png" alt="" class="shadow"/> 

设置推流.2



输入命令行后，暂时先不要点回车，等设置好本地拉流后，再进行推流。

#### 2.本地拉流MP4文件

- 打开VLC播放器

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%2023.43.57.png" alt="" class="shadow"/> 

VLC.3


- 设置播放地址

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%2023.58.54.png" alt="" class="shadow"/>


设置播放地址.4


- 设置拉流地址
    rtmp://localhost:1935/rtmplive/home

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-24%2000.00.14.png" alt="" class="shadow"/>


拉流地址.5


- 开始推流，点击open后开始播放。


<img src="http://7xqmgj.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-23%2023.39.27.png" alt="" class="shadow"/> 

推流成功!

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/%E6%88%AA%E5%9B%BE%202016-07-24%2000%E6%97%B607%E5%88%8621%E7%A7%92.jpg" alt="" class="shadow"/> 

#### 三、用ffmpeg推流桌面以及推流摄像头进行直播

#### 1.如果希望将桌面录制或者分享，可以使用命令行如下：

    ffmpeg -f avfoundation -i "1" -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp://localhost:1935/rtmplive/home


- 这个只能够推桌面。


#### 2.如果需要桌面+麦克风，比如一般做远程教育分享 命令行如下：

    ffmpeg -f avfoundation -i "1:0" -vcodec libx264 -preset ultrafast -acodec libmp3lame -ar 44100 -ac 1 -f flv rtmp://localhost:1935/rtmplive/home


- 这个可以推桌面+麦克风。


#### 3.如果需要桌面+麦克风，并且还要摄像头拍摄到自己，比如一般用于互动主播，游戏主播，命令行如下

    ffmpeg -f avfoundation -framerate 30 -i "1:0" \-f avfoundation -framerate 30 -video_size 640x480 -i "0" \-c:v libx264 -preset ultrafast \-filter_complex 'overlay=main_w-overlay_w-10:main_h-overlay_h-10' -acodec libmp3lame -ar 44100 -ac 1  -f flv rtmp://localhost:1935/rtmplive/home


- 这个可以推桌面+麦克风，并且摄像头把人头放在界面下面

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2004362-1a1e3795c31827c1.png" alt="" class="shadow"/>


Snip20160713_12.png


#### FFmpeg常用基本命令

#### 1.分离视频音频流

    ffmpeg -i input_file -vcodec copy -an output_file_video　　//分离视频流
    ffmpeg -i input_file -acodec copy -vn output_file_audio　　//分离音频流


#### 2.视频解复用

    ffmpeg –i test.mp4 –vcodec copy –an –f m4v test.264ffmpeg –i test.avi –vcodec copy –an –f m4v test.264


#### 3.视频转码

    ffmpeg –i test.mp4 –vcodec h264 –s 352*278 –an –f m4v test.264 //转码为码流原始文件
    ffmpeg –i test.mp4 –vcodec h264 –bf 0 –g 25 –s 352*278 –an –f m4v test.264 //转码为码流原始文件
    ffmpeg –i test.avi -vcodec mpeg4 –vtag xvid –qsame test_xvid.avi //转码为封装文件


- -bf B帧数目控制
- -g 关键帧间隔控制
- -s 分辨率控制


#### 4.视频封装

    ffmpeg –i video_file –i audio_file –vcodec copy –acodec copy output_file


#### 5.视频剪切

    ffmpeg –i test.avi –r 1 –f image2 image-%3d.jpeg //提取图片
    ffmpeg -ss 0:1:30 -t 0:0:20 -i input.avi -vcodec copy -acodec copy output.avi //剪切视频


- -r 提取图像的频率
- -ss 开始时间
- -t 持续时间


#### 6.视频录制

    ffmpeg –i rtsp://192.168.3.205:5555/test –vcodec copy out.avi


#### 7.YUV序列播放

    ffplay -f rawvideo -video_size 1920x1080 input.yuv


#### 8.YUV序列转AVI

    ffmpeg –s w*h –pix_fmt yuv420p –i input.yuv –vcodec mpeg4 output.avi


#### 9.常用参数说明：

**主要参数：**
i 设定输入流
f 设定输出格式
ss 开始时间
**视频参数：**
b 设定视频流量，默认为200Kbit/s-r 设定帧速率，默认为25
s 设定画面的宽与高-aspect 设定画面的比例
vn 不处理视频-vcodec 设定视频编解码器，未设定时则使用与输入流相同的编解码器
**音频参数：**
ar 设定采样率
ac 设定声音的Channel数
acodec 设定声音编解码器，未设定时则使用与输入流相同的编解码器an 不处理音频
 


