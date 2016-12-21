---
layout: post
title: "在AWS EC 2上安装 Ghost "
date: 2016-11-29
author: "Alpaca"
header-img: http://7xqmgj.com1.z0.glb.clouddn.com/2016-11-29-Wallions22023.jpeg
subtitle: "Ghost博客系统"
catalog: true
categories: ios
tags:
   - AWS
   - ghost
      
---

#### Ghost是基于Node.js构建的开源博客平台，所以我们首先搭建Node环境。

    wget http://nodejs.org/dist/v0.10.40/node-v0.10.40.tar.gz  
    tar zxvf node-v0.10.40.tar.gz  
    cd node-v0.10.40  
    ./configure 
    make && make install

若 `configure: error: no acceptable C compiler found` 则 `yum -y install gcc`
若 `没有 GCC 指令` 则 `yum install gcc-c++`
命令执行完毕之后，检测一下环境是否配置成功。

    node -v  
    v0.10.40


*如果显示以上信息，恭喜你，安装成功了~*

#### 安装 nginx 
安装依赖的库：

    yum -y install pcre-devel zlib-devel openssl openssl-devel

安装nginx：

    yum install nginx
    
加入开机自动启动：

    chkconfig --level 35 nginx on
    /etc/init.d/nginx start  
    
 另一种方法:   
 
首先在`/etc/yum.repos.d/`目录下创建一个源配置文件`nginx.repo`：

    vi /etc/yum.repos.d/nginx.repo


写入以下内容：

    [nginx] 
    name=nginx repo  
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/  
    gpgcheck=0  
    enabled=1


保存。(按i编辑，按Esc结束编辑，:x 保存修改并退出，:q! 强制退出，放弃修改)
继续执行以下指令：

    yum install nginx -y # 安装Nginx  
    service nginx start # 启动Nginx  
    chkconfig nginx on # 设置开机启动Nginx


*这样Nginx就安装成功了，在浏览器中输入你的VPS的IP就可以看到提示：“Welcome to Nginx!”*


#### 配置Nginx

安装好了nginx后，我们需要设置一个代理服务器让我们的博客可以使用域名访问。
在`/etc/nginx/conf.d`目录下创建一个配置文件`ghost.conf`：

    vi /etc/nginx/conf.d/ghost.conf


写入以下内容：

    server {  
        listen 80;
        server_name example.com; #将 example.com 改为你的域名或ip。
    
        location / {
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   Host      $http_host;
            proxy_pass         http://127.0.0.1:2368;
        }
    }


保存退出，重启nginx：

    service nginx restart  
 
#### 安装Mysql

Ghost 默认使用 sqlite3 数据库，对于一般使用足够了，但是内容多的话，就会拖慢整个系统，也就影响页面打开速度了，不想使用Mysql的朋友可以跳过这步。
首先输入以下指令:

    yum install mysql mysql-server # 安装Mysql  
    service mysqld start # 启动Mysql  
    chkconfig mysqld on # 设置开机启动Mysql


接着输入mysql_secure_installation配置Mysql：

    Set root password? [Y/n] # 设置root密码  
    anonymous users? [Y/n] # 删除匿名用户  
    Disallow root login remotely? [Y/n] # 禁止root用户远程登录  
    Remove test database and access to it? [Y/n] # 删除默认的 test 数据库  
    Reload privilege tables now? [Y/n] # 刷新授权表使修改生效

为了避免数据库存放的中文是乱码，我们还需要设置Mysql的编码：

    vi /etc/my.cnf


写入以下内容：

    [client]
    default-character-set=utf8  
    [mysql]
    default-character-set=utf8  
    [mysqld]
    character-set-server=utf8  
    collation-server=utf8_general_ci
    
保存退出(exit;)，重启Mysql：

#### 新建一个数据库

    mysql -u root -p # 输入设置好的密码  
    create database ghost; # 创建ghost数据库  
    grant all privileges on ghost.* to 'ghost'@'%' identified by '123456'; # 新建一个用户ghost，密码为123456，随意更改啦  
    flush privileges # 重新读取权限表中的数据到内存，不用重启mysql就可以让权限生效


*这样数据库的准备工作也就完成啦~* 


#### 安装Ghost

做了这么多准备工作，终于要开始折腾我们的主角啦。
首先下载Ghost：

    cd /var/www  
    wget http://dl.ghostchina.com/Ghost-0.7.4-zh-full.zip  
    unzip Ghost-0.7.4-zh-full.zip -d ghost  
    cd ghost


接着修改默认配置：

    cp config.example.js config.js  
    vi config.js


Ghost有产品模式、开发模式和测试模式等多种运行模式，这里我们需要在配置文件中找到production模式：

    # 生产模式
    production: {  
        url: 'http://snowz.me', # 修改为你的域名或者IP，注意加上http://
        mail: {},
        database: {
            client: 'mysql'
            connection: {
                host     : '127.0.0.1',
                user     : 'ghost', # 数据库连接的用户
                password : '123456', # 先前创建的密码
                database : 'ghost', # 先前创建的数据库
                charset  : 'utf8'
            },
        server: {
                host: '127.0.0.1',
                port: '2368' # 若修改该端口记得在nginx中做相应改变
            }
        }


保存退出，接下来就到了见证奇迹的时刻啦，输入指令：

    npm start --production
    
PS: 如果启动出错,我的方法是尝试卸载nodejs 重新安装,并且重新链接npm地址:
卸载安装包方法:
 
    yum remove xxx
查看是否还有mysql软件：

    rpm -qa|grep mysql
    
如果存在的话，继续删除即可。重装mysql也是如此

    npm -v
    bash: /usr/bin/npm: No such file or directory

方法:   

    find / -name "npm-cli.js" // 会显示目录所在
    yourdir -v //显示版本
    ln -s 源目录 目的目录 
    npm -v 
这样应该就可以用了

启动浏览器，输入之前配置的域名或者IP，我们就可以看到建立好的Ghost博客啦。
（Ctrl+C 中断掉开发者模式）

#### 让Ghost保持运行

> 
> 前面提到的启动 Ghost 使用 npm start --production 命令。这是一个在开发模式下启动和测试的不错的选择，但是通过这种命令行启动的方式有个缺点，即当你关闭终端窗口或者从 SSH 断开连接时，Ghost 就停止了。为了防止 Ghost 停止工作，我们得解决这个问题。
> 


以下有几种解决方案：

PM2([https://github.com/Unitech/pm2](https://github.com/Unitech/pm2))
Forever ([https://npmjs.org/package/forever](https://npmjs.org/package/forever))
Supervisor ([http://supervisord.org/](http://supervisord.org/))

这里我们使用PM2让Ghost保持运行：

    cd /var/www/ghost  
    npm install pm2 -g # 安装PM2  
    NODE_ENV=production pm2 start index.js --name "ghost"  
    pm2 startup amazon  
    pm2 save


PS：
天朝可以尝试这时候可以使用以下代码：

    npm install -g cnpm --registry=https://registry.npm.taobao.org  
    cnpm install pm2 -g  
    NODE_ENV=production pm2 start index.js --name "ghost"  
    pm2 startup amazon  
    pm2 save


这样一来，我们的Ghost博客就可以保持运行啦，你可以使用以下指令来控制Ghost博客：

    pm2 start/stop/restart ghost

#### 初始化Ghost

现在所有准备工作都做好了，打开你的浏览器，在浏览器中输入<你的 URL>/ghost/，开始初始化你的Ghost吧~


