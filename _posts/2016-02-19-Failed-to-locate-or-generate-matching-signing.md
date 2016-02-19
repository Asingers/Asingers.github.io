---
layout: post
title: "关于苹果开发证书失效的解决方案"
subtitle: "2016年2月14日Failed to locate or generate matching signing assets"
date: 2016-02-19 
author: "Asingers"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/header_imgWallions10654.jpeg"
categories: ios
tags:
    - iOS
    - Mac
---

前言:
从2月14日开始,上传程序的同学可能会遇到提示上传失败的提示.
并且打开自己的钥匙串,发现所有的证书全部都显示此证书签发者无效.
Failed to locate or generate matching signing assets
Xcode attempted to locate or generate matching signing assets and failed to do so because of the following issues.
Missing iOS Distribution signing identity for ... Xcode can request one for you.


原因 & 解决方法:

为系统导入新的证书.(左上角先打开锁)
下载地址: [https://developer.apple.com/certificationauthority/AppleWWDRCA.cer](https://developer.apple.com/certificationauthority/AppleWWDRCA.cer)

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/post_imgzhengshu.png" alt="" class="shadow"/>

即可


* 后记:
对此苹果官方的解释:
Thanks for bringing this to the attention of the community and apologies for the issues you’ve been having. This issue stems from having a copy of the expired WWDR Intermediate certificate in both your System and Login keychains. To resolve the issue, you should first download and install the new WWDR intermediate certificate (by double-clicking on the file). Next, in the Keychain Access application, select the System keychain. Make sure to select “Show Expired Certificates” in the View menu and then delete the expired version of the Apple Worldwide Developer Relations Certificate Authority Intermediate certificate (expired on February 14, 2016). Your certificates should now appear as valid in Keychain Access and be available to Xcode for submissions to the App Store.

简单点说就是,你颁发开发者证书的根证书失效了,因为他会在2016年2月14日到期.
你之前以此制作的证书才会全部失效.

