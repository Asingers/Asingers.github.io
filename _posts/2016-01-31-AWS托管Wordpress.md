---
layout: post
title: "教程：使用 Amazon Linux 托管 WordPress 博客"
subtitle: "日常搞机"
date: 2016-01-31 16:20:20
author: "Asingers"
header-img: "/img/Wallions10198.png"
categories: aws
tags:
    - AWS
    - EC2
    - 服务器
    - wordpress
---

### 先决条件

本教程假定您已按照教程：安装 LAMP Web 服务器（在 Amazon Linux 上）中的所有步骤，通过支持 PHP 和 MySQL 的功能性 Web 服务器启动了 Amazon Linux 实例。本教程还介绍了配置安全组以允许 HTTP 和 HTTPS 流量的步骤，以及用于确保为 Web 服务器正确设置文件权限的几个步骤。如果您尚未完成这些步骤，请参阅教程：安装 LAMP Web 服务器（在 Amazon Linux 上）以满足这些先决条件，然后回到本教程安装 WordPress。有关添加规则到您安全组的信息，请参阅 向安全组添加规则。

强烈建议您将弹性 IP 地址 (EIP) 与您正用于托管 WordPress 博客的实例关联。这将防止您的实例的公有 DNS 地址更改和中断您的安装。如果您有一个域名且打算将其用于您的博客，则可更新该域名的 DNS 记录，使其指向您的 EIP 地址（如需帮助，请联系您的域名注册商）。您可以免费将一个 EIP 地址与正在运行的实例相关联。有关更多信息，请参阅 弹性 IP 地址。

如果您的博客还没有域名，则可使用 Amazon Route 53 注册一个域名并将您的实例的 EIP 地址与您的域名相关联。有关更多信息，请参阅 Amazon Route 53 开发人员指南 中的使用 Amazon Route 53 注册域名。

### 安装 WordPress

本教程是很好的 Amazon EC2 入门教程，因为您可以完全控制托管您 WordPress 博客的 Web 服务器，这对传统的托管服务来说并不是一个典型的方案。当然，这意味着您要负责更新软件包以及为您的服务器维护安全补丁。对于不需要与 Web 服务器配置直接交互的更自动化 WordPress 安装来说，AWS CloudFormation 服务还会提供可让您快速入门的 WordPress 模板。有关更多信息，请参见 AWS CloudFormation 用户指南 中的入门。如果您更喜欢将您的 WordPress 博客托管在 Windows 实例上，请参阅 Amazon EC2 用户指南（适用于 Microsoft Windows 实例） 中的在您的 Amazon EC2 Windows 实例上部署 WordPress 博客。

### 下载并解压 WordPress 安装包
 1. 使用 wget 命令下载最新 WordPress 安装包。以下命令始终会下载最新版本。 
 
```
 [ec2-user ~]$ wget https://wordpress.org/latest.tar.gz
--2013-08-09 17:19:01--  https://wordpress.org/latest.tar.gz
Resolving wordpress.org (wordpress.org)... 66.155.40.249, 66.155.40.250
Connecting to wordpress.org (wordpress.org)|66.155.40.249|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4028740 (3.8M) [application/x-gzip]
Saving to: latest.tar.gz
100%[======================================>] 4,028,740   20.1MB/s   in 0.2s
2013-08-09 17:19:02 (20.1 MB/s) - latest.tar.gz saved [4028740/4028740]
```

 2. 解压并解档安装包。将安装文件夹解压到名为 wordpress 的文件夹。
 
```
[ec2-user ~]$ tar -xzf latest.tar.gz
[ec2-user ~]$ ls
latest.tar.gz  wordpress
```


  
### 创建 MySQL 用户和数据库以安装 WordPress

安装 WordPress 需要存储信息，例如数据库中的博客文章和用户评论。此步骤将帮助您为自己的博客创建一个数据库，并创建一个有权读取该数据库的信息并将信息保存到该数据库的用户。

1. 启动 MySQL 服务器。

		[ec2-user ~]$ sudo service mysqld start
	
2. 以 root 用户身份登录到 MySQL 服务器。在系统提示时输入您的 MySQL root 密码，这可能与您的 root 系统密码不同，如果您尚未给您的 MySQL 服务器加密，它甚至可能是空的。

	Important:
	
	如果您尚未给您的 MySQL 服务器加密，则必须执行这项操作。有关更多信息，请参阅 保障 MySQL 服务器的安全。
	
		[ec2-user ~]$ mysql -u root -p
		Enter password:

3. 为您的 MySQL 数据库创建用户和密码。安装 WordPress 的过程将使用这些值与您的 MySQL 数据库通信。输入以下命令，以替换唯一的用户名和密码。

		mysql> CREATE USER 'wordpress-user'@'localhost' IDENTIFIED BY 'your_strong_password';
		Query OK, 0 rows affected (0.00 sec)

4. 创建数据库。为数据库提供一个有意义的描述性名称，例如 wordpress-db。
	Note
	
	以下命令中数据库名称两边的标点符号称为反引号。在标准键盘上，反引号 (`) 键通常位于 Tab 键的上方。并不总是需要反引号，但是它们允许您在数据库名称中使用其他的非法字符，例如连字符。
	
		mysql> CREATE DATABASE `wordpress-db`;
		Query OK, 1 row affected (0.01 sec)
5. 对您之前创建的 WordPress 用户授予您数据库的完全访问权限。

		mysql> GRANT ALL PRIVILEGES ON `wordpress-db`.* TO "wordpress-user"@"localhost";
		Query OK, 0 rows affected (0.00 sec)
		

6. 刷新 MySQL 权限以接受您的所有更改。

		mysql> FLUSH PRIVILEGES;
		Query OK, 0 rows affected (0.01 sec)

7. 退出 mysql 客户端。

		mysql> exit
		Bye
### 创建和编辑 wp-config.php 文件

WordPress 安装文件夹包含名为 wp-config-sample.php 的示例配置文件。在本步骤中，您将复制此文件并进行编辑以适合您的具体配置。

1. 将 wp-config-sample.php 文件复制到名为 wp-config.php 的文件。这样做会创建新的配置文件并将原先的示例配置文件原样保留作为备份。

		[ec2-user ~]$ cd wordpress/
		[ec2-user wordpress]$ cp wp-config-sample.php wp-config.php
		
2. 使用常用文本编辑器（如 nano 或 vim）来编辑 wp-config.php 文件，然后输入安装的值。如果您没有喜欢的文本编辑器，nano 对于初学者来说比较容易使用。

		[ec2-user wordpress]$ nano wp-config.php
a.查找定义 DB_NAME 的行并将 database_name_here 更改为您在 创建 MySQL 用户和数据库以安装 WordPress 的 Step 4 中创建的数据库名称。

			define('DB_NAME', 'wordpress-db');
b.查找定义 DB_USER 的行并将 username_here 更改为您在 创建 MySQL 用户和数据库以安装 WordPress 的 Step 3 中创建的数据库用户。

		define('DB_USER', 'wordpress-user');
c.查找定义 DB_PASSWORD 的行并将 password_here 更改为您在 创建 MySQL 用户和数据库以安装 WordPress 的 Step 3 中创建的强密码。

		define('DB_PASSWORD', 'your_strong_password');
d.查找名为 Authentication Unique Keys and Salts 的一节。这些 KEY 和 SALT 值为 WordPress 用户存储在其本地计算机上的浏览器 Cookie 提供了加密层。总而言之，添加长的随机值将使您的站点更安全。访问 https://api.wordpress.org/secret-key/1.1/salt/ 随机生成一组密钥值，您可以将这些密钥值复制并粘贴到 wp-config.php 文件中。要粘贴文本到 PuTTY 终端，请将光标放在您要粘贴文本的地方，并在 PuTTY 终端内部右键单击鼠标。

	有关安全密钥的更多信息，请转至 http://codex.wordpress.org/Editing_wp-config.php#Security_Keys。
   Note:
以下值仅用作示例；请勿使用以下值进行安装。

```
define('AUTH_KEY',         ' #U$$+[RXN8:b^-L 0(WU_+ c+WFkI~c]o]-bHw+)/Aj[wTwSiZ<Qb[mghEXcRh-');
define('SECURE_AUTH_KEY',  'Zsz._P=l/|y.Lq)XjlkwS1y5NJ76E6EJ.AV0pCKZZB,*~*r ?6OP$eJT@;+(ndLg');
define('LOGGED_IN_KEY',    'ju}qwre3V*+8f_zOWf?{LlGsQ]Ye@2Jh^,8x>)Y |;(^[Iw]Pi+LG#A4R?7N`YB3');
define('NONCE_KEY',        'P(g62HeZxEes|LnI^i=H,[XwK9I&[2s|:?0N}VJM%?;v2v]v+;+^9eXUahg@::Cj');
define('AUTH_SALT',        'C$DpB4Hj[JK:?{ql`sRVa:{:7yShy(9A@5wg+`JJVb1fk%_-Bx*M4(qc[Qg%JT!h');
define('SECURE_AUTH_SALT', 'd!uRu#}+q#{f$Z?Z9uFPG.${+S{n~1M&%@~gL>U>NV<zpD-@2-Es7Q1O-bp28EKv');
define('LOGGED_IN_SALT',   ';j{00P*owZf)kVD+FVLn-~ >.|Y%Ug4#I^*LVd9QeZ^&XmK|e(76miC+&W&+^0P/');
define('NONCE_SALT',       '-97r*V/cgxLmp?Zy4zUU4r99QQ_rGs2LTd%P;|_e1tS)8_B/,.6[=UK<J_y9?JWG');
```

   e.保存文件并退出您的文本编辑器。

### 移动 WordPress 安装至 Apache 文档根目录

现在，您已解压了安装文件夹、创建了 MySQL 数据库与用户并自定义了 WordPress 配置文件，那么也就准备好移动您的安装文件至 Web 服务器文档根目录，以便可以运行安装脚本完成安装。这些文件的位置取决于您是希望 WordPress 博客位于 Web 服务器的根目录（例如，my.public.dns.amazonaws.com）还是位于某个子目录或文件夹（例如，my.public.dns.amazonaws.com/blog）中。

 * 选择要在其中提供博客的位置，仅运行与该位置关联的 mv。

	Important
	
	如果同时运行以下两组命令，则在运行第二个 mv 命令时，您会收到一条错误消息，因为您尝试移动的文件已不存在。
 * 要在 my.public.dns.amazonaws.com 中提供博客，请将 wordpress 文件夹中的文件（而不是该文件夹本身）移动到 Apache 文档根目录中（Amazon Linux 实例上的 /var/www/html）。

		[ec2-user wordpress]$ mv * /var/www/html/
	
* 或者，要在 my.public.dns.amazonaws.com/blog 提供博客，请在 Apache 文档根目录中创建名为 blog 的新文件夹，然后将 wordpress 文件夹中的文件（而不是该文件夹本身）移到新的 blog 文件夹中。

		[ec2-user wordpress]$ mkdir /var/www/html/blog
		[ec2-user wordpress]$ mv * /var/www/html/blog

	Important:
	
	出于安全原因，如果您不打算立即进入到下一个过程，请立即停止 Apache Web 服务器 (httpd)。将安装文件移动到 Apache 文档根目录后，WordPress 安装脚本将不受保护，如果 Apache Web 服务器运行，攻击者可能会获得访问您博客的权限。要停止 Apache Web 服务器，请输入命令 sudo service httpd stop。如果您即将继续到下一个步骤，则不需要终止 Apache Web 服务器。

### 允许 WordPress 使用 permalink

WordPress permalink 需要使用 Apache .htaccess 文件才能正常工作，但默认情况下这些文件在 Amazon Linux 上处于禁用状态。使用此过程可允许 Apache 文档根目录中的所有覆盖。

* 使用您常用的文本编辑器（如 nano 或 vim）打开 httpd.conf 文件。如果您没有喜欢的文本编辑器，nano 对于初学者来说比较容易使用。

		[ec2-user wordpress]$ sudo vim /etc/httpd/conf/httpd.conf
		
* 找到以 <Directory "/var/www/html"> 开头的部分。

```
	#<Directory "/var/www/html">
    #
    # Possible values for the Options directive are "None", "All",
    # or any combination of:
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    #
    # Note that "MultiViews" must be named *explicitly* --- "Options All"
    # doesn't give it to you.
    #
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.4/mod/core.html#options
    # for more information.
    #
    Options Indexes FollowSymLinks
    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   Options FileInfo AuthConfig Limit
    #
    AllowOverride None
    #
    # Controls who can get stuff from this server.
    #
    Require all granted
	#</Directory>
```

* 在以上部分中将 AllowOverride None 行改为读取 AllowOverride All。

	Note
	此文件中有多个 AllowOverride 行；请确保更改 <Directory "/var/www/html"> 部分中的行。
	
		AllowOverride All
* 保存文件并退出您的文本编辑器。

### 修复 Apache Web 服务器的文件权限

WordPress 中的某些可用功能要求具有对 Apache 文档根目录的写入权限 (例如通过“Administration (管理)”屏幕上传媒体)。Web 服务器以 apache 用户身份运行，因此，您需要将该用户添加至在 wwwLAMP Web 服务器教程中创建的 组。

1. 将 apache 用户添加到 www 组。

		[ec2-user wordpress]$ sudo usermod -a -G www apache
		
2. 将 /var/www 及其内容的文件所有权更改到 apache 用户。

		[ec2-user wordpress]$ sudo chown -R apache /var/www
3. 将 /var/ 及其内容的组所有权更改到 wwwwww 组。

		[ec2-user wordpress]$ sudo chgrp -R www /var/www
4. 更改 /var/www 及其子目录的目录权限，以添加组写入权限和设置未来子目录上的组 ID。

		[ec2-user wordpress]$ sudo chmod 2775 /var/www
		[ec2-user wordpress]$ find /var/www -type d -exec sudo chmod 2775 {} +
5. 递归更改 /var/www 及其子目录的文件权限，以添加组写入权限。

		[ec2-user wordpress]$ find /var/www -type f -exec sudo chmod 0664 {} +
6. 重启 Apache Web 服务器，让新组和权限生效。

		[ec2-user wordpress]$ sudo service httpd restart
		Stopping httpd:                                            	[  OK  ]
		Starting httpd:                                            	[  OK  ]
		
### 运行 WordPress 安装脚本
1. 使用 chkconfig 命令确保 httpd 和 mysqld 服务在每次系统启动时启动。

		[ec2-user wordpress]$ sudo chkconfig httpd on
		[ec2-user wordpress]$ sudo chkconfig mysqld on
2. 验证 MySQL 服务器 (mysqld) 正在运行。

		[ec2-user wordpress]$ sudo service mysqld status
		mysqld (pid  4746) is running...
3. 如果 mysqld 服务未运行，请启动。

		[ec2-user wordpress]$ sudo service mysqld start
		Starting mysqld:                                           [  OK  ]
4. 验证您的 Apache Web 服务器 (httpd) 正在运行。

		[ec2-user wordpress]$ sudo service httpd status
		httpd (pid  502) is running...
5. 如果 httpd 服务未运行，请启动。

		[ec2-user wordpress]$ sudo service httpd start
		Starting httpd:                                            [  OK  ]
6. 在 Web 浏览器中，输入您 WordPress 博客的 URL（您实例的公有 DNS 地址，或者该地址后跟 blog 文件夹）。您应该可以看到 WordPress 安装屏幕。

	http://my.public.dns.amazonaws.com
![](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/images/wordpress_install.png)
7. 将其余安装信息输入 WordPress 安装向导。

	字段	 | 值 | 
	------------ | ------------- | 
	Site Title (网站标题)	 | 为您的 WordPress 网站输入名称。  | 
	Username | 为您的 WordPress 管理员输入名称。出于安全原因，您应为此用户选择一个唯一名称，因为与默认用户名称 admin 相比，该名称更难破解。  | 
	密码 | 输入强密码，然后再次输入进行确认。请勿重复使用现有密码，并确保将密码保存在安全的位置。 |
	Your E-mail (您的电子邮件) | 输入您用于接收通知的电子邮件地址。 |
	
8. 单击 Install WordPress (安装 WordPress) 完成安装。

恭喜您，您现在应该可以登录您的 WordPress 博客并开始发布博客文章。



