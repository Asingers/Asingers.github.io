---
layout: post
title: "解决Sourcetree 每次拉取提交都需要输入密码"
subtitle: "解决方案 Mac保存密码"
date: 2016-02-25 
author: "Alpaca"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/header_imgnsarry.jpg"
categories: ios
tags:
    - iOS
    - Dev
---

**git config --global credential.helper osxkeychain**

如果不能执行,请先安装:


	$ git credential-osxkeychain
	# Test for the cred helper
	  git: 'credential-osxkeychain' is not a git command. See 'git --help'.
	$ curl -s -O \
	  https://github-media-downloads.s3.amazonaws.com/osx/git-credential-osxkeychain
	# Download the helper

	$ chmod u+x git-credential-osxkeychain
	# Fix the permissions on the file so it can be run


	$ sudo mv git-credential-osxkeychain \
	  "$(dirname $(which git))/git-credential-osxkeychain"
	# Move the helper to the path where git is installed
	  Password: [enter your password]

	$  git config --global credential.helper osxkeychain
	# Set git to use the osxkeychain credential helper
	
相关问题,可以看[stackoverflow](http://stackoverflow.com/questions/11745504/remove-credential-osxkeychain)解答
