---
layout: post
title: "基于Github和Octopress搭建属于自己的博客"
date: 2016-01-26 14:10:07 +0800
comments: true
categories: Life
---

# 安装Octopress
[这里](http://octopress.org/docs/setup/)是Octopress的官方指南，各位可以按照其中的步骤进行安装，下面的文字只是记录了我个人的安装过程，可以为大家提供一些参考。

由于Octopress的使用需要Ruby，于是搭环境就这条路上的第一只拦路虎。Ruby版本繁多并且版本之间向下兼容做的不好，所以基于Ruby所做的框架大多要求特定版本的Ruby才能正常运行。

Octopress要求的是Ruby1.9.3，MacOSX自带的Ruby版本是2.x的，所以需要利用一些工具来安装低版本的Ruby。Octopress的官方指南推荐使用的是RVM和rbenv，可以根据需要选择使用需要的工具。

### 安装rbenv

在第一次安装octopress（OSX 10.10时代）的过程中我首先使用了RVM，但碰到了一些莫名的问题无法解决，最后还是使用了rbenv。

这里我使用[Homebrew](http://brew.sh/)来安装rbenv，如果你没有Homebrew，打开终端，使用以下命令安装吧。


	$ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"


有了Homebrew就可以安装rbenv了


	$ brew update
	$ brew install rbenv
	$ brew install ruby-build


### 用rbenv安装Ruby
使用rbenv安装1.9.3版本的ruby，一开始我安装的是1.9.3-p0的版本，但出现了一些错误，在搜素如何解决的过程中发现[@Peter潘](http://www.cnblogs.com/peterpan507/p/3538057.html)在blog中写到可以尝试用1.9.3-p125，经过尝试成功的安装上了Ruby1.9.3-p125

安装完成后可以用ruby --version进行验证


	$ rbenv install 1.9.3-p125
	$ rbenv local 1.9.3-p125
	$ rbenv rehash
	$ ruby --version #ruby 1.9.3p125 (2012-02-16 revision 34643)

### 安装RVM

在第二次安装octopress（OSX 10.11时代）的过程中依照之前的经验使用rbenv，但却怎么也安不上需要的ruby版本，最后换回了RVM。。

安装octopress官方提供的安装方法


	$ curl -L https://get.rvm.io | bash -s stable --rub

### 用RVM安装Ruby
使用rvm安装1.9.3版本的ruby，我依照之前的经验安装1.9.3-p125的版本，但在编译时出现了一些错误，继而尝试了评论中[@lance_lan]()提到得1.9.3-p551版本才安装成功。

	$ rvm install ruby-1.9.3-p551 --with-gcc=clang
	$ rvm use 1.9.3-p551
	$ rvm rubygems latest
	$ ruby --version #ruby 1.9.3p551 (2014-11-13 revision 48407)
###安装Octopress
安装Ruby完成后就按照官方指南安装Octpress


# clone octopress
	$ git clone git://github.com/imathis/octopress.git octopress
	$ cd octopress

# 安装依赖

	$ gem install bundler
	$ rbenv rehash  # 如果你刚才用了rbenv
	$ bundle install

# 安装octopress默认主题

	$ rake install

---------

# 部署
接下来需要把Blog部署到github上去，第一步要做的是去[github](https://github.com/new)创建一个`username.github.io`的repo，比如我的就叫`msching.github.io`。

然后运行以下命令，并依照提示完成github和Octopress的关联


	$ rake setup_github_pages

---------

# 创建博客

### 生成博客

	$ rake generate
	$ rake deploy


把生成后的代码上传到github


	$ git add .
	$ git commit -m 'create blog'
	$ git push origin source

完成后等待一段时间后就能访问`http://username.github.io`看到自己的博客了


### 修改配置
配置文件路径为`~/octopress/_config.yml`


	url:                # For rewriting urls for RSS, etc
	title:              # Used in the header and title tags
	subtitle:           # A description used in the header
	author:             # Your name, for RSS, Copyright, Metadata
	simple_search:      # Search engine for simple site search
	description:        # A default meta description for your site
	date_format:        # Format dates using Ruby's date strftime syntax
	subscribe_rss:      # Url for your blog's feed, defauts to /atom.xml
	subscribe_email:    # Url to subscribe by email (service required)
	category_feeds:     # Enable per category RSS feeds (defaults to false in 2.1)
	email:              # Email address for the RSS feed if you want it.

编辑完成后

	$ rake generate
	$ git add .
	$ git commit -m "settings" 
	$ git push origin source
	$ rake deploy
	
### 安装第三方主题
Octopress有许多第三方主题可以选择，首先在[这里](http://opthemes.com/)上寻找喜欢的主题，点击进入对应主题的git，一般在readme上都会有安装流程


# 这里以安装allenhsu定制的greyshade主题为例，原作者是shashankmehta

	$ git clone git@github.com:allenhsu/greyshade.git .themes/greyshade

	Substitue 'color' with your highlight color
	$ echo "\$greyshade: color;" >> sass/custom/_colors.scss 
	$ rake "install[greyshade]"
	$ rake generate

	$ git add .
	$ git commit -m "theme" 
	$ git push origin source

	$ rake deploy


### 定制第三方主题

使用第三方主题也并非是一个“拎包入住”的过程，其中必然会有一些需要定制的地方。定制的过程中会涉及一些web相关的知识，但对于各位来说应该都并非难事。

例如刚安装完greyshade之后我们会发现左边navigation上的**About me**是指向作者的个人主页，我们需要把这个文字连接定向到自己的个人主页上。

这个aboutme对应的html为`/source/_includes/custom/navigation.html`


	<li><a href="{{ root_url }}/">Blog</a></li>
	<li><a href="http://about.me/shashankmehta">About</a></li>
	<li><a href="{{ root_url }}/blog/archives">Archives</a></li>

把其中的`http://about.me/shashankmehta`替换成需要的url，替换完之后


	$ rake generate
	$ git add .
	$ git commit -m "theme" 
	$ git push origin source

	$ rake deploy


### 支持中文标签
目前版本的Octopress会在`/source/blog/categories`下创建一个`index.markdown`来作为分类的首页，但这个首页在标签有中文时会出现无法跳转的情况，原因是因为在出现中文标签时Octopress会把文件的路径中的中文转换成拼音，而在Category跳转时是直接写了中文路径，结果自然是404。解决方法是自己实现一个分类首页并处理中文。

首先按照[这里](https://kaworu.ch/blog/2013/09/23/categories-page-with-octopress/)的方法实现`index.html`

将`plugins/category_list_tag.rb`中的


	category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)


替换成

	category_url = File.join(category_dir, category.to_url.downcase)

这样你的博客就可以支持中文标签的跳转了。



# 写博客

经过上面几部后，博客已经成功搭建，现在就可以开始写博文了。

### 创建博文

# 如果用的是终端
	$ rake new_post['title']

# 如果用的是ZSH
	$ rake "new_post[title]"
# 或者
	$ rake new_post\['title'\]

生成的文件在`~/source/_posts`目录下


### 编辑博文


# markdown写博文

	$ rake preview #localhost:4000

	$ rake generate

	$ git add .
	$ git commit -m "comment" 
	$ git push origin source

	$ rake deploy



# 参考资料

* http://octopress.org/
* http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/
* http://www.cnblogs.com/peterpan507/p/3538057.html
* http://biaobiaoqi.me/blog/2013/03/21/building-octopress-in-github-mac/
* http://biaobiaoqi.me/blog/2013/07/10/decorate-octopress/
* http://yanping.me/cn/blog/2012/01/07/theming-and-customization/