---
layout: post
title: "使用Jenkins搭建持续集成服务"
subtitle: "Jenkins Service"
date: 2016-03-01
author: "Alpaca"
categories: ios
tags:
    - iOS
    - Dev
    - Mac
    - Jenkins
---


# 使用Jenkins搭建持续集成服务


## 1. 什么是持续集成

**持续集成**(Continuous Integration, 简称 CI) 是软件工程中的一种实践,
用于将开发人员不同阶段的工作成果集成起来, 通常一天之中会进行多次. 持续集成最初在
**极限编程**(Extreme Programming) 中提出, 主要用于[执行](http://www.extremeprogramming.org/rules/integrateoften.html)[自动化测试](http://www.extremeprogramming.org/rules/unittests.html).
目前持续集成的概念已经逐渐独立出来, 并扩展为**构建服务器**(Build Server),
**质量控制**(Quality Control) 和**持续交付**(Continuous Delivery)
等多种形式和实践. 详细可以参见[Wikipedia](http://en.wikipedia.org/wiki/Continuous_integration).

下面我们就看一下如何搭建[Jenkins](http://jenkins-ci.org/)(一个基于Java的持续集成工具) 并用于执行自动化测试.
我们要达到这样的效果: 在向位于[BitBucket](https://bitbucket.org/)的项目 push 代码时, Jenkins
自动获取最新的代码并执行测试, 并将测试结果通过Email或其他方式通知我们.

另外, 本文也会介绍几个持续集成服务商, 可以免去自己安装和维护的工作.
详见[持续集成服务商](#ci-sass).

## 2. 安装和配置Jenkins

下面介绍如何在Ubuntu系统上安装和配置Jenkins. 如果你对Ubuntu系统不熟悉, 可以参考
[Linux服务器的初步配置流程](http://www.ruanyifeng.com/blog/2014/03/server_setup.html), 那里有如何配置 sudo 等信息.

### 2.1 安装Jenkins

详细的安装步骤可以参考[官方文档](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu), 主要步骤如下:


    wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key|sudo apt-key add -
    sudo sh -c&#39;echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list&#39;sudo apt-get update
    sudo apt-get install jenkins



这样就会自动安装Jenkins和它的依赖(如OpenJDK), 并自动在 8080 端口启动Jenkins.
如果8080端口已被占用则会启动失败, 可以关闭占用8080端口的程序, 或者更改Jenkins的配置使用其他端口(见后文).

为了执行项目自动化测试而需要的软件和工具可以在这里一并安装和配置, 例如我的项目需要
Rvm, Ruby 和[PostgreSQL](http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html).

### 2.2 Jenkins命令

以下是用于管理Jenkins服务的常用命令:

- 查看Jenkins是否正在运行:`sudo service jenkins status`
- 运行Jenkins:`sudo service jenkins start`
- 停止Jenkins:`sudo service jenkins stop`
- 重启Jenkins:`sudo service jenkins restart`


### 2.3 Jenkins运行选项

Jenkins默认使用的配置文件位于*/etc/default/jenkins*, 在这里可以更改Jenkins的运行选项.
例如, 如果要修改内存大小和运行端口, 可以使用`sudo vi /etc/default/jenkins`
打开这个文件然后修改这两个选项:

    JAVA_ARGS="-Xmx512m"
    HTTP_PORT=9080


修改配置文件后记得重启Jenkins:`sudo service jenkins restart`

### 2.4 Jenkins安全配置

接下来在浏览器打开*host:port*(例如 http://localhost:8080) 就可以访问到Jenkins的页面了,
其中*host*是你的服务器IP或域名,*port*是Jenkins的运行端口(默认是8080).
你也可以配合Nignx或Apache来使用80端口, 以及设置SSL等.

第一次运行Jenkins时要做的特别重要的一件事就是**配置安全选项**,
也就是访问和修改Jenkins的权限. 默认的设置是所有人拥有所有的权限.
我们来改成需要使用用户名和密码登录.

首先创建一个用户. 从左边的菜单区依次进入*Manage Jenkins*,*Manage Users*,*Create User*,
然后填入自己的登录信息.

之后回到首页, 依次进入*Manage Jenkins*,*Configure Global Security*,
勾上*Enable security*和*Jenkins’ own user database*, 去掉*Allow users to sign up*
前面的勾.

接下来, 如果你的Jenkins是内部使用, 不想公开给大众, 可以使用*Matrix-based security*,
在*User/group to add:*后面输入刚才创建的用户名并点*Add*, 然后在出现的那一行勾上
*Administer*(*Anonymous*那一行都不要勾上).

而如果你想允许没登录的人查看Jenkins(只读权限), 可以使用*Logged-in users can do anything*.

以上是典型的安全配置, 你也可以根据自己的情况决定如何配置.

## 3. Jenkins Job 示例: 自动化测试

由于我们的示例需要使用 Git 和 Bitbucket, 先做一些必要的安装和配置.

### 3.1 安装和配置 Git 及 ssh key

首先安装Git:`sudo apt-get install git`

然后切换到`jenkins`用户, 下面的命令都要以这个用户的身份执行:


    sudo su jenkins



为`jenkins`用户配置 Git 用户名和email (根据你的情况进行修改):


    git config --global user.name"Jenkins"git config --global user.email"jenkins@example.org"



然后用`ssh-keygen`命名生成 ssh key. 可以一路回车, 默认生成的 ssh public key
位于*~/.ssh/id_rsa.pub*. 详细可以参见[SSH原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html).

然后运行一次`ssh git@bitbucket.org`并输入`yes`, 这样BitBucket就会被自动加到
*known_hosts*里了.

然后需要把 ssh public key 加到BitBucket项目的*Deployment keys*, 这样`jenkins`
用户才有权限 clone 项目代码. 执行`cat ~/.ssh/id_rsa.pub`并复制输出的 public key
内容 (注意不要复制多余的空行), 然后打开BitBucket项目管理页面的*Deployment keys*页面,
粘贴到*Key*文本框里,*Label*建议填写`jenkins ci`, 然后点击*Add key*.

最后, 需要给Jenkins安装Git插件. 在Jenkins页面依次选择*Manage Jenkins*,*Manage Plugins*,
*Available*, 找到并勾上*GIT plugin*. 由于我的项目是使用了Rvm, 所以我也会勾上
*Rvm*. 然后点击页面最下方的*Install without restart*.

### 3.2 创建Job

下面我们就来创建用于执行自动化测试的Jenkins Job.

在Jenkins页面点击*New Item*, 在*Item name*输入Job名称, 例如*my-project-rspec*,
点击*OK*.

在*Source Code Management*区域勾选*Git*,*Repository URL*填写项目在BitBucket的地址,
例如`git@bitbucket.org:user/repo.git`. 在*Branches to build*后面填写想要对哪个分支持续集成,
我们留空来针对所有分支.

在*Build Triggers*区域勾选*Poll SCM*(*Schedule*和*Ignore post-commit hooks*留空).

如果要使用 Rvm 插件, 在*Build Environment*区域勾选*Run the build in a RVM-managed environment*,
并在*Implementation*后面填写你在 Rvm 中使用的 Ruby 版本, 例如`2.0.0-p451`.

在*Build*区域依次选择*Add build step*,*Execute shell*, 然后在*Command*
右边的文本框输入执行持续集成的脚本, 例如为我的项目执行自动化测试:


    bundle installexportRAILS_ENV=testbundleexecrake db:schema:load
    bundleexecrspec spec



最后, 点击*Save*, 回到刚创建好的Job的页面, 点击*Build Now*, 查看Build的*Console Output*,
如果遇到错误就做相应的修改, 之后再*Build Now*直到成功.

### 3.3 配置自动触发

现在我们已经可以手动触发持续集成来运行测试了, 下面配置在 push 代码时自动触发.

进入BitBucket项目管理页面的*Hooks*页面, 在*Select a hook...*选择*POST*
并点击*Add hook*, 然后在*URL*填写*JENKINS_URL/git/notifyCommit?url=GIT_REPO*,
其中*JENKINS_URL*是你的Jenkins地址,*GIT_REPO*是在上一步填写的Git仓库地址.
例如:

    http://localhost:8080/git/notifyCommit?url=git@bitbucket.org:user/repo.git


然后点*Save*. 以后当有代码push到BitBucket项目时, Jenkins就会自动触发执行测试.

### 3.4 配置通知

最后, 我们需要配置通知以便在集成失败时得到通知. 如果你使用[HipChat](https://www.hipchat.com/)或[Slack](https://slack.com/)
那么建议你直接使用他们接收通知, 只需安装对应的插件和简单的配置即可.

下面我们介绍如何让Jenkins发送Email通知. 首先需要配置SMTP. 依次进入*Manage Jenkins*,
*Configure System*, 在*E-mail Notification*区域进行设置. 以Gmail为例:
*SMTP server*输入`smtp.gmail.com`, 点击*Advanced...*, 勾选*Use SMTP Authentication*,
*User Name*和*Password*输入 Gmail 邮箱地址和密码, 勾选*Use SSL*,
*SMTP Port*填写`465`. 然后可以勾选*Test configuration by sending test e-mail*
并填写自己的Email地址再点*Test configuraton*进行测试, 如果可以收到测试邮件则说明配置成功.
然后点击页面底部的*Save*.

接下来进入Job页面, 点击*Configure*进入Job的配置页面, 在最下面的*Add post-build action*
选择*E-mail Notification*, 然后在*Recipients*输入接收通知邮件的邮箱地址,
多个地址之间用英文空格隔开 (建议使用邮件列表地址).

## 4. 持续集成服务商

以上就是Jenkins持续集成服务器的基本配置和使用.
除了某些情况下必须像这样自己搭建持续集成服务器外, 其实可以考虑一些[SaaS](http://en.wikipedia.org/wiki/Software_as_a_service)
类型的持续集成服务, 例如[Travis CI](https://travis-ci.org/),[CircleCI](https://circleci.com/),[Codeship](https://www.codeship.io/)和[Drone](https://drone.io/).
此类的服务对于开源项目基本都是免费的.
对于私有项目的收费可能也会比自己搭建和维护持续集成服务器的成本更低, 值得考虑.

以下是我对这几个持续集成服务的初步印象:

*[Travis CI](https://travis-ci.org/): 在GitHub项目中非常流行,[收费版](https://travis-ci.com/plans)的起步价($129)比较高.
*[CircleCI](https://circleci.com/): 只支持GitHub项目. 听说速度很快.
*[Codeship](https://www.codeship.io/): 网站界面很漂亮. 我在使用中遇到两个不常见的问题: 在用`git push -f`
覆盖了之前失败的一次commit时, Codeship会由于找不到之前的commit而无法继续执行;
另外那个触发自动执行的hook URL只在刚开始创建项目时可以看到, 之后在设置页面找不到,
这样如果在BitBucket/GitHub临时删除了hook就找不回来了. 这两个问题貌似只能通过删除并重建项目来解决.

*[Drone](https://drone.io/): 基于[Docker](https://www.docker.io/). 我在使用中发现的一个问题是, 每次执行都会从零开始构建环境,
例如我的Ruby项目, 每次执行`bundle install`都重新安装所有依赖导致用时太长,
暂时没有研究有无解决方法.

最后, 关于自己搭建持续集成服务, 除了Jenkins还有其他类似的工具, 例如[GitLab CI](https://www.gitlab.com/gitlab-ci/),
[ThoughtWorks Go](http://www.go.cd/),[Drone](https://github.com/drone/drone)(开源版) 等, 可以参考[Wikipedia](http://en.wikipedia.org/wiki/Continuous_integration#Software)
和[开源中国社区](http://www.oschina.net/project/tag/344/ci)上的列表.

