---
layout: post
title: "Jekyll 安装"
subtitle: "傻瓜模式"
date: 2016-02-04 
author: "Alpaca"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/header_imgjekyll-guide.jpg"
categories: jekyll
tags:
    - Jekyll
---


安装是一个很简单的过程。但是有时候因为你的电脑的一些配置，前提环境的问题，又会变得很复杂。

这里先介绍一下安装 Jekyll 的前提要求：

- 需要有`Ruby`
- 在`Ruby`的基础上，安装好`RubyGems`
- 操作系统要求：Linux, Unix, 或者 Mac OS


### 一. 在本地安装 Ruby

详细的安装文档，可以查看 Ruby 官方的[安装介绍](http://www.ruby-lang.org/en/downloads/)。我在这里列出几种简单的方法，便于快速参考。

使用 RVM 安装。关于 RVM 安装的详细方法在[Installation page](https://rvm.io/rvm/install)。
 
	## 首先要先安装 RVM
	$ \curl -L https://get.rvm.io | bash
 
	## 再安装 Ruby   	
	$ rvm install 1.9.2
    
	## 使用 Ruby
	$ rvm use 1.9.2
	
Linux 下的安装方法。在终端上执行：

	$ sudo apt-get install ruby1.9.1

Mac 下使用[Homebrew](http://brew.sh/)来安装，挺方便的。

	$ brew install ruby


安装完之后，在终端检查是否安装好。运行`ruby -v`，看看输入的日志是否为`ruby 1.9.3p327`，后面的版本号跟你安装的版本有关。目前 Jekyll 最新版本需要 Ruby 1.9.1 以上。

### 二. 安装 RubyGems

[RubyGems](http://rubygems.org/pages/download)是一个 Ruby 包的管理工具，就像 Homebrew，npm 等，可以下载包到本地。

下载 RubyGems 安装包，当前是 2.0.6 版本：[tgz](http://production.cf.rubygems.org/rubygems/rubygems-2.0.6.tgz)-[zip](http://production.cf.rubygems.org/rubygems/rubygems-2.0.6.zip)-[gem](http://production.cf.rubygems.org/rubygems/rubygems-update-2.0.6.gem)-[git](http://github.com/rubygems/rubygems)。安装到本地之后，在终端检查更新：

    ##可能需要根权限
    $ gem update --system
    
    ##检查当前版本，输出 2.0.6
    $ gem -v


### 三. 安装 Jekyll

最好的安装方法应该是通过 RubyGems 来安装，在终端输入：

    ##可能需要根权限    
    $ gem install jekyll
    
    ##安装完了之后，查看版本号，现在打印出来的是 jekyll 11.2
    
    $ jekyll -v


至此，你已经成功在本地电脑上安装好了 Jekyll。

### 四. 我遇到的安装问题

但是这个过程中，可能会遇到很多问题，例如 Ruby 安装不来，例如 Jekyll 安装不好。

使用`brew`安装 Ruby 的时候，居然遇到了 404 ！

	$ brew install ruby
    	
	==> Downloading http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p327.tar.gz
    
	curl: (22) The requested URL returned error: 404
    	
	Error: Download failed: http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p327.tar.gz


**解决方法**：通过 FTP 工具把这个 Ruby 包下载下来，然后放到 Homebrew 的 Cache 文件夹中，路径是：`/Library/Caches/Homebrew`。然后再通过 brew 安装，就可以了。

- 安装成功之后，终端运行 Ruby 没生效


**解决方法**：这个很简单，因为安装成功之后，没有把目录添加到 PATH 里面。Mac 下设置环境变量在`~/.bash_profile`里面。添加完之后，运行：

    $ source .bash_profile


安装 Jekyll 成功，但是运行时报这个错误:

	ERROR: YOUR SITE COULD NOT BE BUILT:
           ------------------------------------
           Missing dependency: rdiscount


**解决方法**：这里的`rdiscount`可以是 Jekyll 依赖的一个包，可以通过安装这个包来解决。

    ##和 Jekyll 一样使用 gem 来装，需要根权限
    
    $ gem install rdiscount

> 原文地址 http://www.zhanxin.info/jekyll/2013-08-07-jekyll-doc-installation.html


