---
layout: post
title: "微信支付遇到跳转只有一个确定的问题"
date: 2016-05-09
author: "Asingers"
subtitle: "你可能遇到的问题"
catalog: true
categories: ios
tags:
   - iOS
---

问题如图所示:

<img src="http://cdn.cocimg.com/bbs/attachment/Fid_21/21_124147_0a10442f326939b.jpg" alt="" class="shadow"/> 

首先确认 是传递接口的字段导致的问题。是传递接口的字段导致的问题。是传递接口的字段导致的问题。

说几个可能出现问题的点。
第一步获取prepayId，这一步往往都不会有什么错误，根着官方文档都不会出现什么问题，坑在第二步发送跳转


1、注意一下 nonceStr 需要是第一步里生成的 nonceStr，而不是重新生成。



2、sign 需要重新针对5个字段进行签名：partnerId prepayId package nonceStr timeStamp  不需要传入appid或者openid
需要传入appid

3、package = @"Sign=WXPay" 注意服务器传来的"="会不会被转义成 %3D


4、sign的确需要大写，不像之前有些帖子说的要小写。


[@狂龙天使 的demo地址](http://www.cocoachina.com/bbs/read.php?tid-309177-keyword-%CE%A2%D0%C5%D6%A7%B8%B6.html)

内容更新：
by luohuasheng0225
我补充一坑：
1、如果你app同时使用了友盟分享（含微信分享）和微信支付。如果你没有处理好这个两个SDK register的顺序，那就很不幸，也会出现这种情况。
（如何出现这种情况，请看我的测试步骤：1、杀掉微信进程、2、删除自己开发的app、3、重新同步自己的app到设备，点击微信支付）  

两者register的顺序：如果是先调用微信registerApp、然后调用友盟的 [UMSocialWechatHandler setWXAppId:WXAppID appSecret:[NSString stringWithBundleNameForKey:@"WXAppSecret"] url:url] ，然后按照我测试的步骤，应该就会出现。
解决办法：改变两者的register步骤。先调用友盟，然后调用微信。 

内容更新：
by yutiandesan
补充一点，时间戳需要为10位，之前后台给的是13位，也是只有一个确定按钮，并且ret=-2