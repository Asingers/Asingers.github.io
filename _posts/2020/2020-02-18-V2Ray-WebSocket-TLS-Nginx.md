---
layout: post
title: V2Ray+WebSocket+TLS+Nginx
description: V2Ray+WebSocket+TLS+Nginx 一键搞定
categories: tips
tags: AWS

---
### 安装
脚本适用于：Debian 9+ / Ubuntu 18.04+ / Centos7+

Vmess+websocket+TLS+Nginx+Website

`bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh) | tee v2ray_ins.log`

安装过程需要输入一个域名地址，需要将VPS 的 IP 地址解析到你的域名，二级域名也可以，解析域名与安装配置域名一致即可，安装脚本配置端口选择443（默认）。

基本就是喝点水等着就行。安装成功相应的网站也可以访问了，也就是前边配置的域名。

Mac 客户端： [V2rayU](https://github.com/yanue/V2rayU/releases)

ps:脚本管理命令

启动 V2ray：

`systemctl start v2ray`

停止 V2ray：

`systemctl stop v2ray`

启动 Nginx：

`systemctl start nginx`

停止 Nginx：

`systemctl stop nginx`

脚本相关目录
Web 目录：

`/home/wwwroot/levis`

V2ray 服务端配置：

`/etc/v2ray/config.json`

V2ray 客户端配置：

`执行安装时所在目录下的 v2ray_info.txt`

Nginx 目录：

`/etc/nginx`

证书目录：

`/data/v2ray.key 和 /data/v2ray.crt`
