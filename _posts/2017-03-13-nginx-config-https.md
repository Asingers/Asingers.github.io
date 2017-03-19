---
layout: post
title: "使用免费 SSL 证书 Let's Encrypt(certbot) 搭建 Https"
date: 2017-03-13
author: "Asingers"
header-img: http://7xqmgj.com1.z0.glb.clouddn.com/2016-11-29-Wallions22023.jpeg
subtitle: "学习笔记"
catalog: true
categories: life
tags:
   - AWS
      
---

下载文件:

	wget https://dl.eff.org/certbot-auto --no-check-certificate
	
	chmod +x ./certbot-auto
	
	./certbot-auto --debug  也有可能是 ./certbot-auto certonly 进行手动配置
Nginx:

需要验证权限,nginx conf 中加入:  

	location ~ /\.well-known/acme-challenge/ {
           root /usr/local/ispconfig/interface/acme/;
           index index.html index.htm;
           try_files $uri =404;
	}
	
然后根据提示输入相关信息生成证书

最后配置 nginx:

	server {
        listen       80;
        server_name  xxx.com;

        location / {
            rewrite ^(.*) https://$server_name$1 permanent;
        }
    }

    # HTTPS server
    server {
        listen       443 ssl;
        server_name  xxx.com;

        ssl_certificate      /etc/letsencrypt/live/xxx.com/fullchain.pem;
        ssl_certificate_key  /etc/letsencrypt/live/xxx.com/privkey.pem;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
    }

Apache: 

httpd.conf中 引入:

	Include extra/vhost.conf
	
httpd/extra 中增加 vhost.con 配置:  
	
	<VirtualHost *:443>
    DocumentRoot /var/www/html
    ServerName asingers.win:443
    ServerAlias www.asingers.win
    DirectoryIndex index.html index.php
    ErrorLog logs/main-error_log
    CustomLog logs/main-access_log common
	</VirtualHost>
	
获取证书:  

	sudo ./certbot-auto --debug certonly --webroot --webroot-path /var/www/html --renew-by-default --email xx@qq.com --text --agree-tos -d asingers.win 
	
	
	
