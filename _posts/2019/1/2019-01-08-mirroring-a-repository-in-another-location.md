---
layout: post
title: Mirroring a repository in another location
subtitle: 镜像一个仓库到另一个地方
categories: iOS
header-mask: 0.7
tags: 
    - iOS
    - Git

---

### clone 源地址
If you want to mirror a repository in another location, including getting updates from the original, you can
clone a mirror and periodically push the changes.

*Create a bare mirrored clone of the repository.*
	
	$ git clone --mirror https://github.com/exampleuser/repository-to-mirror.git

### Set the push location to your mirror. 设置目的地地址

	$ cd reposi tory- to-mi rror. git
	$ git remote set-url --push origin https://github.com/exampleuser/mi rrored
	
### Push

As with a bare clone, a mirrored clone includes all remote branches and tags, but all local references
will be overwritten each time you fetch, so it will always be the same as the original repository. Setting
the URL for pushes simplifies pushing to your mirror. To update your mirror, fetch updates and push.

	$ git fetch -p origin
	$ git push --mirror
	


