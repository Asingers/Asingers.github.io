---
layout: post
title: 更优雅的方式在 AWS 上跑 Shadowsocks
subtitle: 外面的世界很精彩~
author: "Asingers"
categories: ios
header-mask: 0.3
tags: 
    - Shadowsocks
    - docker
    - AWS
---


#### 准备

1. Linux服务器
2. 安装 Docker ```yum install -y docker ```并启动```sudo service docker start ```
3. 一键安装并启动  

```docker run -d --restart=always --name=ss-libev-port8388 -p 8388:54321 -e PASSWORD='123456' -e METHOD=aes-256-cfb --cap-add=NET_ADMIN jokester/shadowsocks-libev```  
停止:```docker rm -f ss-libev-port8388```  


其中 `8388`为可选端口,`123456`为可选密码,`aes-256-cfb`为可选加密方式.

完了.   


	
