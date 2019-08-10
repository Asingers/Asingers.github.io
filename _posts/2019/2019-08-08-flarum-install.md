---
layout: post
title: Flarum Install
description: Flarum Install Guide
categories: iOS
tags: GCP

---
*  首先创建实例。SSH 连接之后切换到root 用户，sudo -i 


*  为了方便网站管理推荐安装宝塔面板[宝塔](https://www.bt.cn/bbs/thread-19376-1-1.html)。


*  在宝塔面板中搭建LAMP环境，请选择下述配置，并以快速安装的方式进行安装。（安装开始后将持续1.5小时，无需值守）。


`Apache 2.4.25
MySQL 5.7.19
Pure-Ftpd 1.0.47
PHP 7.2
PhpMyAdmin 4.4`

*  在宝塔面板中选择 软件管理 - PHP7.2 - 安装扩展 安装下列扩展。

`fileinfo 扩展
opcache 扩展
exif 扩展`

然后在 禁用函数 中删掉下方选项。`proc_open`

*  在宝塔面板中选择 网站 然后 添加站点。

*  第二部分 Flarum安装

安装Composer，可以按照官方指导就可以了[Composer安装](https://getcomposer.org/download/)


*  安装Flarum
*  
这个步骤其实也可以在宝塔面板通过创建站点的方式创建文件夹，终端安装可能需要通过以下命令（需要在一个空文件夹里）
`php composer.phar create-project flarum/flarum ./web --stability=beta`

安装可能会报错：
`[ErrorException]                                 
  putenv() has been disabled for security reasons`
	需要在面板的PHP设置中不要禁用这个函数。
	
*  在宝塔面板中点击 网站 - 网站名 - 网站目录，将目录地址更改为 /www/wwwroot/web/ 等对应地址就可以了。并点击保存。运行目录更改为/public 并点击保存。

*  给文件夹授权，在SSH中设置运行下列命令

`chmod -R 0777 /www/wwwroot/web/storage`

`chmod -R 0777 /www/wwwroot/web/public/assets`


*  第三部分 配置Flarum

输入域名或者IP 地址即可开始初始化安装，相关配置参数就是创建站点时候生成的，比如数据库名密码。

*  为网站设置SSL，在宝塔面板中，选择网站 - 你的域名 - 弹出设置窗口后，在SSL面板位置，申请一个宝塔SSL，然后在此期间你需要保证网站正常运行。
当你的SSL证书申请下来之后，点击部署，然后打开强制HTTPS，然后在宝塔面板中选择文件面板，进入 /www/wwwroot/flarum 目录下，有个config.php文件，编辑它。
第16行有你的域名 http://xxx.com 这样的，请将 http:// 改为https:// ，然后再次访问你的网站，SSL安全锁就出来了。

这是安装官方版本的方法，当然也可以通过上传安装包的方式，可以从[汉化版](https://www.flarumchina.org/docs/installation/)下载，然后文件解压到网站目录下。也可以进行安装。

