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

先使用 homebrew 安装 Jenkins


    $brew install jenkins



然后链接 launchd 配置文件


    $ln -sfv /usr/local/opt/jenkins/*.plist ~/Library/LaunchAgents



可以更改此 plist 来进行一些自定义的配置，详细列表可以参考[https://wiki.jenkins-ci.org/display/JENKINS/Starting+and+Accessing+Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Starting+and+Accessing+Jenkins)

> 
> 如果要其他机器也可以访问，把 plist 里的`<string>--httpListenAddress=127.0.0.1</string>`删掉即可
> 


修改完后，在终端执行


    $launchctl load ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist



即可启动 Jenkins

接着用浏览器访问`localhost:8080`（默认配置），就可以看到 Jenkins 的 web 界面了

	# 这种方式安装的Jenkins默认目录是/usr/local/Cellar/jenkins/1.651/libexec/....
	#所以想让其他局域网用户访问则需要修改/etc/apache2/httpd.conf的ServerRoot 路径  
	改为/usr/local/Cellar/ 即可 
	 
重启 Apache 

	sudo apachectl restart


### 集成 GitHub 的 Pull Request

在 GitHub 上有新的 Pull Request 的时候，可以自动来跑测试，然后把结果提交给 GitHub 上

#### 安装插件

在左侧的导航找到`Manage Jenkins`，进到管理界面，然后找到`Manage Plugins`进入插件管理界面，我们安装几个必须的插件：

- GitHub plugin[https://wiki.jenkins-ci.org/display/JENKINS/Github+Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Github+Plugin)
- GitHub Pull Request Builder[https://wiki.jenkins-ci.org/display/JENKINS/GitHub+pull+request+builder+plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitHub+pull+request+builder+plugin)


#### Job 跑起来

安装完插件之后，我们可以开始建立 Job，在首页的左侧找到 New Job 进入新建 Job 的界面，选择`Build a free-style software project`

![Screen Shot 2013-11-25 at 2.00.36 PM.png](http://user-image.logdown.io/user/749/blog/746/post/162202/vDclj1BQR9qvMPAhQhdG_Screen%20Shot%202013-11-25%20at%202.00.36%20PM.png)

点击 OK 之后，进入配置界面，找到 Source Code Management，按照下图，选择 Git，然后填入 Name 为`origin`，Refspec 填入`+refs/pull/*:refs/remotes/origin/pr/*`，Branch Specifier 填入`${sha1}`

![Screen Shot 2013-11-25 at 2.02.52 PM.png](http://user-image.logdown.io/user/749/blog/746/post/162202/g62kVG4sRqykiRynCBIu_Screen%20Shot%202013-11-25%20at%202.02.52%20PM.png)

接着在 Build Trigger 下选择上`GitHub pull requests builder`，默认使用的是 polling，这里也可以配置使用 github 的 hook 来触发，具体可以点击旁边的问号查看帮助

![Screen Shot 2013-11-25 at 2.10.02 PM.png](http://user-image.logdown.io/user/749/blog/746/post/162202/SYHWaxR9QdG14iNmpXeJ_Screen%20Shot%202013-11-25%20at%202.10.02%20PM.png)

接着在 Build 下，添加`Execute shell`的 build step，里面写上跑测试的脚本

![Screen Shot 2013-11-25 at 2.16.21 PM.png](http://user-image.logdown.io/user/749/blog/746/post/162202/2u14UI1PQb6lakztF97K_Screen%20Shot%202013-11-25%20at%202.16.21%20PM.png)

[格志](http://griddiaryapp.com)里使用了 cocoapods，还有用 bundler 来管理 cocoapods 和其他 Ruby Gems 的版本，所以我们使用下面这段脚本来跑测试


    #!/usr/bin/env zsh --loginrvm use 2.0.0
    ruby --version
    bundle
    bundle --version
    bin/pod
    /usr/bin/xcodebuild -scheme'GridDiary Beta'-workspace GridDiary.xcworkspace -destination"platform=iOS Simulator,name=iPhone Retina (4-inch),OS=latest"-configuration Debug clean buildtestONLY_ACTIVE_ARCH=NO



最后在 Post-build Actions 里加入`Set build status on GitHub commit`

### Extra

Jenkins 默认的界面惨不忍睹，可以用[Simple Theme](https://wiki.jenkins-ci.org/display/JENKINS/Simple+Theme+Plugin)这个插件来来自定 CSS 和 JavaScript

还有一个可以让 console 更不费眼睛一点的 Chrome 插件：[https://github.com/M6Web/JenkinsTerminalColors](https://github.com/M6Web/JenkinsTerminalColors)
