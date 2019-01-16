---
layout: post
title: Publish your opensource cocoapods
subtitle: 制作并发布一个开源库
categories: iOS
header-mask: 0.7
tags: 
    - iOS
    - Git
    - Cocoapods
---

### Github上创建自己的仓库
这个不用多说，你可以创建自己的项目丢一些东西进去，可以是一个工程，后续将决定你把那一部分内容开源出去。

### 提交自己的变更
打上tag备用

	//删除本地tag 
	# git tag -d 标签名  
	// 删除远程tag
	# git push origin :refs/tags/标签名
	
	git tag "v1.0.0"  
    git push --tags
    
### 注册

	pod trunk me
	
若未注册，执行以下命令，邮箱以及用户名请对号入座。

	pod trunk register example@example.com 'example'  --verbose
	
### 创建.podspec
配置文件
	
	pod spec create yourname
	
eg：

	#  Be sure to run `pod spec lint YJTouchID.podspec' to ensure this is a
	#  valid spec and to remove all comments including this before submitting the spec.

	#  To learn more about Podspec attributes see http://docs.cocoapods.org/specification.html
	#  To see working Podspecs in the CocoaPods repo see https://github.com/CocoaPods/Specs/

	Pod::Spec.new do |s|

	  # ―――  Spec Metadata  ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	#
	  #  These will help people to find your library, and whilst it
	  #  can feel like a chore to fill in it's definitely to your advantage. The
	  #  summary should be tweet-length, and the description more in depth.
	  #

	  s.name         = "YJTouchID"
	  s.version      = "0.0.2"
	  s.summary      = "A Easy Way To Use YJTouchID."

	  # This description is used to generate tags and improve search results.
	  #   * Think: What does it do? Why did you write it? What is the focus?
	  #   * Try to keep it short, snappy and to the point.
	  #   * Write the description between the DESC delimiters below.
	  #   * Finally, don't worry about the indent, CocoaPods strips it!
	  s.description  = "快速集成TouchID 到当前项目中。A Easy Way To Use YJTouchID😄"

	  s.homepage     = "https://github.com/Asingers/YJTouchID"
	  # s.screenshots  = "www.example.com/screenshots_1.gif", "www.example.com/screenshots_2.gif"


	  # ―――  Spec License  ――――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	  #
	  #  Licensing your code is important. See http://choosealicense.com for more info.
	  #  CocoaPods will detect a license file if there is a named LICENSE*
	  #  Popular ones are 'MIT', 'BSD' and 'Apache License, Version 2.0'.
	  #

	  # s.license      = "MIT"
	  s.license      = { :type => "MIT", :file => "LICENSE" }


	  # ――― Author Metadata  ――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	  #
	  #  Specify the authors of the library, with email addresses. Email addresses
	  #  of the authors are extracted from the SCM log. E.g. $ git log. CocoaPods also
	  #  accepts just a name if you'd rather not provide an email address.
	  #
	  #  Specify a social_media_url where others can refer to, for example a twitter
	  #  profile URL.
	  #

	  s.author             = { "zhangyuanjie" => "309954331@qq.com" }
	  # Or just: s.author    = "zhangyuanjie"
	  # s.authors            = { "zhangyuanjie" => "309954331@qq.com" }
	  # s.social_media_url   = "http://twitter.com/zhangyuanjie"

	  # ――― Platform Specifics ――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	  #
	  #  If this Pod runs only on iOS or OS X, then specify the platform and
	  #  the deployment target. You can optionally include the target after the platform.
	  #

	  # s.platform     = :ios
	  s.platform     = :ios, "9.0"

	  #  When using multiple platforms
	  # s.ios.deployment_target = "5.0"
	  # s.osx.deployment_target = "10.7"
	  # s.watchos.deployment_target = "2.0"
	  # s.tvos.deployment_target = "9.0"


	  # ――― Source Location ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	  #
	  #  Specify the location from where the source should be retrieved.
	  #  Supports git, hg, bzr, svn and HTTP.
	  #

	  s.source       = { :git => "https://github.com/Asingers/YJTouchID.git", :tag => "#{s.version}" }


	  # ――― Source Code ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	  #
	  #  CocoaPods is smart about how it includes source code. For source files
	  #  giving a folder will include any swift, h, m, mm, c & cpp files.
	  #  For header files it will include any header in the folder.
	  #  Not including the public_header_files will make all headers public.
	  #

	  s.source_files  = "YJTouchID", "YJTouchID/YJTouchID/*.{h,m}"
	  # s.exclude_files = "Classes/Exclude"

	  # s.public_header_files = "Classes/**/*.h"


	  # ――― Resources ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――	― #
	  #
	  #  A list of resources included with the Pod. These are copied into the
	  #  target bundle with a build phase script. Anything else will be cleaned.
	  #  You can preserve files from being cleaned, please don't preserve
	  #  non-essential files like tests, examples and documentation.
	  #

	  # s.resource  = "icon.png"
	  s.resources = "YJTouchID/pic/*.png"

	  # s.preserve_paths = "FilesToSave", "MoreFilesToSave"


	  # ――― Project Linking 	―――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	  #
	  #  Link your library with frameworks, or libraries. Libraries do not include
	  #  the lib prefix of their name.
	  #

	  # s.framework  = "SomeFramework"
	  # s.frameworks = "SomeFramework", "AnotherFramework"

	  # s.library   = "iconv"
	  # s.libraries = "iconv", "xml2"


	  # ――― Project Settings ――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
	  #
	  #  If your library depends on compiler flags you can set them in the xcconfig hash
	  #  where they will only apply to your library. If you depend on other Podspecs
	  #  you can include multiple dependencies to ensure it works.

	  # s.requires_arc = true

	  # s.xcconfig = { "HEADER_SEARCH_PATHS" => "$(SDKROOT)/usr/include/libxml2" }
	  # s.dependency "JSONKit", "~> 1.4"

	end
	
* s.name：名称，pod search搜索的关键词,注意这里一定要和.podspec的名称一样,否则报错

* s.version：版本号，to_s：返回一个字符串

* s.author:作者

* s.homepage:项目主页地址

* s.summary: 项目简介

* s.source:项目源码所在地址

* s.license:许可证

* s.platform:项目支持平台

* s.requires_arc: 是否支持ARC

* s.source_files:需要包含的源文件

* s.public_header_files:需要包含的头文件

* s.ios.deployment_target:支持的pod最低版本
* s.social_media_url:社交网址
* s.resources:资源文件
* s.dependency:依赖库，（一些第三方）不能依赖未发布的库

source_files写法及含义：

	"AA/*
	"AA/BB/*.{h,m}"
	"AA/**/*.h"
	
	*表示匹配所有文件
	*.{h,m}表示匹配所有以.h和.m结尾的文件
	**表示匹配所有子目录

### 验证.podspec

	pod spec lint yourname.podspec --verbose --use-libraries
	
### 发布

	pod trunk push yourname.podspec --use-libraries --allow-warnings

要注意每次有新的代码push之后，要打上将要发布的最新版的tag,例如下次更新了1.0.1

	git tag 1.0.1
	git push --tags
	
修改yourname.podspec文件中的内容

	s.version      = "1.0.1"
	s.source       = { :Git => "https://github.com/xxx/xxx.git", :tag => "1.0.1" }
	
这样就能对应到版本了，然后执行下边命令就能搜到你的库了

	rm ~/Library/Caches/CocoaPods/search_index.json
	pod search yourname
	
