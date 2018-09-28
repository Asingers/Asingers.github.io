---
layout: post
title: Mac Iterm Terminal Proxy Setup
subtitle: Mac终端配置代理
categories: Mac
header-mask: 0.7
tags: 
    - iOS
    - Git
    - Mac

---

由于某些原因，终端访问github被禁止，所以需要为终端配置代理，当然前提是您可以正常使用SS，直接进入主题⬇️

### 安装

	brew install proxychains-ng
	
### 配置

编辑配置文件 vim /usr/local/etc/proxychains.conf
在末尾的 [ProxyList] 下加入代理类型

	socks5 127.0.0.1 1080 //注意端口匹配
	
### 使用

	proxychains4 xxx
	
### 注意⚠️
1.如果您在使用更新的Mac 需要关闭sip：重启电脑cmd+r 终端输入*csrutil disable* 然后*reboot*

2.如果遇到 LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to xx:443,需要在终端输入
	
	git config --global http.proxy 'socks5://127.0.0.1:1086'
	git config --global https.proxy 'socks5://127.0.0.1:1086'
	同样需要注意端口匹配
	
恢复默认：

	git config --global --unset http.proxy
	git config --global --unset https.proxy
	npm config delete proxy
3.如果遇到 pod repo update failed: Cannot do hard reset with paths 请使用⬇️

	proxychains4 -q pod repo update

