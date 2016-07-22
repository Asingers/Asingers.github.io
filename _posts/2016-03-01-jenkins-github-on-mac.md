---
layout: post
title: "在Mac下搭建Jenkins Github环境"
subtitle: "Jenkins+Github"
date: 2016-03-01
author: "Asingers"
categories: ios
tags:
    - iOS
    - Dev
    - Mac
    - Jenkins
---


**1, 下载安装版[http://jenkins-ci.org/](http://jenkins-ci.org/)**(注: 默认端口为8080)  
 
或者通过命令行安装:  

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

    sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist 启动  
       
    sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist 停止
    

如果要修改端口，比如7070，可在第8步重启jenkins前执行以下命令修改端口参数

    defaults write /Library/Preferences/org.jenkins-ci httpPort 7070


**2，安装后可直接访问[http://localhost:8080](http://localhost:8080/)**

**3，安装github插件**

进入到Jenkins->Manage Jenkins->Manage plugins搜索github并安装[GitHub plugin](http://wiki.jenkins-ci.org/display/JENKINS/Github+Plugin)即可

**4, 开启用户权限**

- 选中Jenkins->Manage Jenkins->Configure Global Security->Enable Security->Jenkins's own user database->Allow users to sign up
- 选中Jenkins->Manage Jenkins->Configure Global Security->Enable Security->Project-based Matrix Authorization Strategy


**5, 在Project-based Matrix Authorization Strategy下添加两个用户分别是admin和github，参考如下**

![http://dl.iteye.com/upload/attachment/0078/9100/aca7b10e-f62f-3d1b-a590-31e3f865af43.png](http://dl.iteye.com/upload/attachment/0078/9100/aca7b10e-f62f-3d1b-a590-31e3f865af43.png)

*注：admin全选，github只要选择Read项即可*

**6，创建与上面匹配的账户**

选择Jenkins->Manage Jenkins->Manage Users->Create User分别创建用户名为admin和github的账户并到第五步的页面查看是否生效，如果生效，github前面的禁止警告标记将变成人形图标，类似admin前面的图标一样。

**7，创建一个用户名为jenkins的影身账户，用户主目录设置在/Users/Shared/Jenkins/Home**

    sudo dscl . create /Users/jenkins
    sudo dscl . create /Users/jenkins PrimaryGroupID 1
    sudo dscl . create /Users/jenkins UniqueID 300  
    sudo dscl . create /Users/jenkins UserShell /bin/bash
    sudo dscl . passwd /Users/jenkins $PASSWORD
    sudo dscl . create /Users/jenkins home /Users/Shared/Jenkins/Home/
    sudo chown -R jenkins: /Users/Shared/Jenkins/Home


**8，编辑/Library/LaunchDaemons/org.jenkins-ci.plist，修改username为jenkins**

重启jenkins:

    sudo launchctl unload -w /Library/LaunchDaemons/org.jenkins-ci.plist
    sudo launchctl load -w /Library/LaunchDaemons/org.jenkins-ci.plist


**9, 将已有的id_rsa和id_rsa.pub放入到/Users/Shared/Jenkins/Home/.ssh中（如果没有则需要按照github.com的文档重新创建）。**

在这之后你应该就可以clone你的代码了，但是如果你之前为自己的id_rsa设置了passphrase，则每次clone都需要你输入，这个显然会破坏jenkins自动下载源码，我的做法是使用***ssh-keygen -p***命令将passphrase置空（这个是偷懒的办法，正确的做法应该是把passphrase保存起来避免重复输入）

**10，使用下面的命令设置你的github信息**

    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"


**11，在Jenkins->Manage Jenkins->Configure System里配置邮件发送信息，参考如下**


![http://dl.iteye.com/upload/attachment/0078/9102/e7dc2bb2-aef3-3939-91aa-24114d70274f.png](http://dl.iteye.com/upload/attachment/0078/9102/e7dc2bb2-aef3-3939-91aa-24114d70274f.png)

最后记得将Jenkins->Manage Jenkins->Configure System->System Admin e-mail address里的配置更新成***Jenkins-CI <your@email.com>***

**12，两种方式实现有push到github操作的时候主动触发jenkins**

方式一(Push)：参考[http://nepalonrails.com/post/14217655627/set-up-jenkins-ci-on-ubuntu-for-painless-rails3-app-ci](http://nepalonrails.com/post/14217655627/set-up-jenkins-ci-on-ubuntu-for-painless-rails3-app-ci)

方式二(Pull)：对你的Job进行如下配置


![http://dl.iteye.com/upload/attachment/0078/9259/4f7de6aa-a801-3224-b5d0-43edbc237538.png](http://dl.iteye.com/upload/attachment/0078/9259/4f7de6aa-a801-3224-b5d0-43edbc237538.png)

**13，做完以上就可以添加JOB进行CI集成了。**

参考：

[http://nepalonrails.com/post/14217655627/set-up-jenkins-ci-on-ubuntu-for-painless-rails3-app-ci](http://nepalonrails.com/post/14217655627/set-up-jenkins-ci-on-ubuntu-for-painless-rails3-app-ci)
