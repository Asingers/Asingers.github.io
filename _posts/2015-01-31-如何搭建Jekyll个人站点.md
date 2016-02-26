---
layout: post
title: "如何搭建Jekyll个人站点"
subtitle: "快速搭建"
date: 2015-01-31 11:30:30
author: "Asingers"
categories: Life
tags: Jekyll 
---


# <center>如何搭建Jekyll个人站点</center>
    
> 这个教程更针对从没接触过git的朋友,所以大神可以不用往下看了.

首先我们来看几个,例子.

* 第一个例子[Clickhere](http://travelog.io/)
* 第二个例子[Clickhere](http://mikevormwald.com/joon/)

好了,上边两个都是Jekyll官方给出的Demo.如果你希望有一个个人网站,博客,接下来我们要做的就是,一步一步搭建一个免费个人博客.
首先先引入几个概念:Git,Github,Jekyll.没关系,今天我们暂时不需要对他们进行深入了解.我们今天要做的就是使用Jekyll搭建博客并上传到Github仓库中.然后就可以通过访问域名的方式查看自己的博客.

# 创建仓库
Github和Gitcafe等都可以托管代码,考虑到中文和国内访问的原因,我们今天暂时使用Gitcafe提供的空间作为我们的仓库.

### 创建账户

1.登陆[Gitcafe](https://gitcafe.com)创建账号

<img src="http://7xoawu.com1.z0.glb.clouddn.com/gude-1.png" alt="" class="shadow" />

2.点击新建,创建一个新的仓库,也就是我们的博客所有文件存储的地方.
<img src="http://7xoawu.com1.z0.glb.clouddn.com/Snip20160102_2.png" alt="" class="shadow" />

3.这一步很关键,仓库名称要与我们的用户名相同,勾选图示,你会发现设置相同之后,会自动为你填充描述等.如图所示:
<img src="http://7xoawu.com1.z0.glb.clouddn.com/guide3.jpg" alt="" class="shadow" />

4.接下来,我们点击仓库名称,进行下一步设置.
<img src="http://7xoawu.com1.z0.glb.clouddn.com/guide-4.png" alt="" class="shadow" />

5.这一步,我们是要拿到自己仓库的地址.复制粘贴备用,如图所示
<img src="http://7xoawu.com1.z0.glb.clouddn.com/Snip20160102_4.png" alt="" class="shadow" />

6.添加密钥
  这一步我们需要为我们的账户添加密钥,这样我们就能在本地和远程仓库直接建立联系,为后续工作做准备.如图所示,电子头像进入账户设置-添加公钥
  <img src="http://7xoawu.com1.z0.glb.clouddn.com/guide-6.png" alt="" class="shadow" />
  
7.新建密钥,首先需要到[Git下载中心](http://git-scm.com/download/)下载对应系统的Git安装包.然后安装,备用.今天我们拿Mac来做例子.安装之后,打开我们电脑的终端(Windows打开桌面Git图标就行).
这里放一个[Gitcafe官方添加公钥教程](https://help.gitcafe.com/manuals/help/ssh-key),描述的非常详细,我就不再赘述.或这你看一下我操作的例子,注意途中标红的位置,就是主要的命令.

添加公钥:
  <img src="http://7xoawu.com1.z0.glb.clouddn.com/Snip20160102_3.png" alt="" class="shadow" />
  
其中,cd+空格+路径名,就是进入到哪个文件夹的意思;ls就是列出文件下所有内容;cd+空格+..,就是向上一级的意思,这三个命令你至少要熟记,尽管你之前可能没接触也没用过.如下图:
  <img src="http://7xoawu.com1.z0.glb.clouddn.com/guide-7.png" alt="" class="shadow" />

好,当我们按照教程添加好公钥之后,接下来就要对仓库进行仓库了.

### 克隆仓库
  <img src="http://7xoawu.com1.z0.glb.clouddn.com/guide.gif" alt="" class="shadow" />
  
1.如图,我们操作的意思是:进入到桌面,在进入一个叫MyJekyllGuide的文件,这个无所谓,就是本地博客文件的路径,你当然也可以越简单越好,直接cd desktop,就行了,意思就是下一步我们把远程仓库克隆到桌面上,会自动生成一个文件的.
需要用到的命令就是:git clone +仓库地址,注意空格.还记得我们之前复制粘贴的那个仓库地址吗,就是我们在这里需要的.你可以反复看Gif演示的来操作.
2.好,等克隆完成之后,你会放现在你存放的路径下有一个文件夹,那个就是我们在网页中新建的仓库的内容,新的仓库只有一个文件,不用管,接下来我们要做的是,在仓库中生成一个分支.

  <img src="http://7xoawu.com1.z0.glb.clouddn.com/guide-2.gif" alt="" class="shadow" />

如图所示,用的的命令是:git checkout -b gitcafe-pages,注意空格.你会发现,操作完之后文件夹里内容并没有什么变化,对的,我们只是生成了一个新的分支,接下来就是要往我们的仓库里放东西,也就是我们的博客文件.

3.Jekyll模板.Jekyll官方提供了很多模板免费使用,作为例子,你可以先下载这个[testTheme](https://codeload.github.com/rowanoulton/galileo-theme/zip/master)使用,当然你也可以到[Jekyll Theme](http://jekyllthemes.org/)挑选喜欢的.

下载好之后,解压,将所有文件放到我们的本地仓库中,就是我们克隆之后在相应路径生成的那个文件夹里.类似下图:
  <img src="http://7xoawu.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-01-02%2020.02.57.png" alt="" class="shadow" />

4.Push.接下来要做的就是将本地仓库中添加的东西推到远程仓库中,也就是服务器中,这样我们的博客文件才能够被编译生成网页.
作为例子,我就将这篇正在写的内容上传上去.如下图:

  <img src="http://7xoawu.com1.z0.glb.clouddn.com/push.gif" alt="" class="shadow" />

其中,我们用到三个命令.当然命令可以不同也能实现相同效果,但是怎么简单怎么来,如下图所示:注意箭头所指位置.
1.git add -A . 添加所有内容(注意空格和最后的.)
2.git commit -m 'sss' 添加log信息,其中sss就是我们所需要附加的信息,就是每次更新的说明性内容,这个任意写,比如aaa就行
3.git push -u origin gitcafe-pages 推送到远程仓库的相对应gitcafe-pages分支.
4.当然每次都输入者三个重复的命令实在太不明智了,你可以下载我的这个[脚本下载](http://9dic.com/download/gitcafePush.sh),拖进终端,直接回车就一部搞定了,不需要敲三部,如下图:

 <img src="http://7xoawu.com1.z0.glb.clouddn.com/push-2.gif" alt="" class="shadow" />

这个步骤做完,我们的初级目标就达成了,你可以尝试在浏览器中输入"xxx.gitcafe.io",xxx是你的gitcafe用户名.有没有发现我们放进去的那个testTheme模板已经显示出来.

 <img src="http://7xoawu.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-01-02%2020.34.47.png" alt="" class="shadow" />

好,至此,我们告一段落.关于如何定制自己的主题,和更多操作,包括发表博客,域名绑定,我们稍后再续.
