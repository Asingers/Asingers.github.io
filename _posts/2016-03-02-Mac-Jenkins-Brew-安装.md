---
layout: post
title: "Mac Jenkins Brew 安装"
subtitle: "Jenkins Brew Way"
date: 2016-03-02
author: "Asingers"
categories: ios
tags:
    - iOS
    - Dev
    - Linux
    - AWS
---
### 安装配置 Jenkins

通过命令行安装:  

    brew cask install jenkins  
    
前提是已经安装Java  

    brew cask install java  
    
安装成功会自动启动并打开网页  
    
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2008%E6%97%B646%E5%88%8621%E7%A7%92.png" alt="" class="shadow"/>   

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2008%E6%97%B647%E5%88%8616%E7%A7%92.png" alt="" class="shadow"/>   

启动执行脚本目录所在:  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2008%E6%97%B658%E5%88%8648%E7%A7%92.png" alt="" class="shadow"/>  

安装包所在:  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2009%E6%97%B602%E5%88%8606%E7%A7%92.png" alt="" class="shadow"/>
服务项所在:  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2009%E6%97%B604%E5%88%8610%E7%A7%92.png" alt="" class="shadow"/> 
可以通过命令开始停止:  

    // 方法一:
    sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist 启动  
       
    sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist 停止
    

如果要修改端口，比如7070，可在第8步重启jenkins前执行以下命令修改端口参数

    defaults write /Library/Preferences/org.jenkins-ci httpPort 7070

Jenkins默认安装目录:  

    /users/share/  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2009%E6%97%B627%E5%88%8623%E7%A7%92.png" alt="" class="shadow"/>  

或者更改目录:  

    cd 到 /Library/LaunchDaemons 编辑 org.jenkins-ci.plist  更改jenkinshome和username
    重启Jenkins即可 





然后链接 launchd 配置文件
    
    // 方法二(1): 
    $ln -sfv /usr/local/opt/jenkins/*.plist ~/Library/LaunchAgents



可以更改此 plist 来进行一些自定义的配置，详细列表可以参考[https://wiki.jenkins-ci.org/display/JENKINS/Starting+and+Accessing+Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Starting+and+Accessing+Jenkins)

> 
> 如果要其他机器也可以访问，把 plist 里的`<string>--httpListenAddress=127.0.0.1</string>`删掉即可
> 


修改完后，在终端执行

    // 方法二(2)
    $launchctl load ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist



即可启动 Jenkins

接着用浏览器访问`localhost:8080`（默认配置），就可以看到 Jenkins 的 web 界面了

	# 这种方式安装的Jenkins默认目录是/usr/local/Cellar/jenkins/1.651/libexec/....
	#所以想让其他局域网用户访问则需要修改/etc/apache2/httpd.conf的ServerRoot 路径  
	改为/usr/local/Cellar/ 即可 

进入 系统管理-启用安全-访问控制-Jenkins专有用户数据库-安全矩阵 添加一个用户:  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2009%E6%97%B650%E5%88%8651%E7%A7%92.png" alt="" class="shadow"/>  

保存之后会在Jenkins安装目录下生成config.xml文件.  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-22_%E6%88%AA%E5%9B%BE%202016-07-22%2010%E6%97%B603%E5%88%8620%E7%A7%92.png" alt="" class="shadow"/>  


    <useSecurity>true</useSecurity>  这个节点表示使用安全管理，也就是需要用户登录才能操作  
    
用刚才添加的用户进行注册,不使用密码登录可以  

    <useSecurity>false</useSecurity>   
    
即可.