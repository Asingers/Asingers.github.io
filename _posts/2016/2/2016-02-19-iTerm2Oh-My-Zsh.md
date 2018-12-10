---
layout: post
title: "iTerm 2 Oh My Zsh"
subtitle: "DIY教程"
date: 2016-02-19 
author: "Alpaca"
categories: ios
tags:
    - iOS
    - Mac
---

先来看一下效果:

![](http://7xqmgj.com1.z0.glb.clouddn.com/post_imgiterm22.png)


### 1. 首先下载[iTerm 2](http://www.iterm2.com/)

### 2. 打开iTerm 2

### 3. 输入下面指令安装[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

`curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh`

### 4. 接下来安装[Powerline](http://powerline.readthedocs.org/en/latest/installation.html)

在官网有教程，我们只需要执行官网第一条安装指令就行

如果你的终端能够正常执行pip指令，那么直接执行下面的指令可以完成安装

`pip install powerline-status`

如果没有，则先执行安装pip指令

`sudo easy_install pip`

### 5. 下载、安装库[字体库](https://github.com/powerline/fonts)

1）将工程下载下来后cd到`install.sh`文件所在目录

2）执行指令安装字体库

执行`./install.sh`指令安装所有Powerline字体

安装完成后提示所有字体均已下载到`/Users/superdanny/Library/Fonts`路径下

    All Powerline fonts installed to /Users/superdanny/Library/Fonts


### 6. 设置iTerm 2的Regular Font 和 Non-ASCII Font

安装完字体库之后，把iTerm 2的设置里的Profile中的Text选项卡中里的Regular Font和Non-ASCII Font的字体都设置成 Powerline的字体，我这里设置的字体是12pt Meslo LG S DZ Regular for Powerline

<img src="http://upload-images.jianshu.io/upload_images/645592-eafa2148c1755383.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240/format/jpg" alt="" class="shadow"/>


设置iTerm 2


### 7. 配色方案

1）安装[配色方案](https://github.com/altercation/solarized)

进入刚刚下载的工程的solarized/iterm2-colors-solarized下双击Solarized Dark.itermcolors和Solarized Light.itermcolors两个文件就可以把配置文件导入到 iTerm2 里

2）配置配色方案

通过load presets选择刚刚安装的配色主题即可


<img src="http://upload-images.jianshu.io/upload_images/645592-00c72100725f2407.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240/format/jpg" alt="" class="shadow"/>


配色方案


### 8. 使用agnoster主题

1）下载[agnoster](https://github.com/fcamblor/oh-my-zsh-agnoster-fcamblor)主题
到下载的工程里面运行install文件,主题将安装到~/.oh-my-zsh/themes目录下

2）设置该主题
进入~/.zshrc打开.zshrc文件，然后将ZSH_THEME后面的字段改为agnoster。ZSH_THEME="agnoster"（agnoster即为要设置的主题）

### 9. 增加指令高亮效果——[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

指令高亮效果作用是当用户输入正确命令时指令会绿色高亮，错误时命令红色高亮

1）cd到.zshrc所在目录

2）执行指令将工程克隆到当前目录

git clone git://github.com/zsh-users/zsh-syntax-highlighting.git

3）打开.zshrc文件，在最后添加下面内容

source XXX/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

保存文件。

注意：xxx代表.zshrc所在目录

4）cd ~/.oh-my-zsh/custom/plugins

5）再次打开.zshrc文件，在最后面添加下面内容

plugins=(zsh-syntax-highlighting)

保存文件。

---

## F Q

1. 启动iTerm 2 默认使用dash改用zsh解决方法：
chsh -s /bin/zsh
2. 执行指令pip install powerline-status出错解决方法：
需要下载苹果官方的[Command line](https://developer.apple.com/downloads/index.action?name=for%20Xcode%20)。必須官方工具下载最新版 Command Line
3. ⌘+Q关闭iTerm 2 时每次弹窗提示问题：
iTerm 2 中，进入Preference-General-Closing栏目，将Confirm "Quit iTerm2(⌘Q)" command选项勾选去掉就行
4. 通常zshrc文件在 /user/youname 下
