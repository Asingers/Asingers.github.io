---
layout: post
title:  "GithubPages 上 HTTPS"
date:   2016-03-17
author:     "Asingers"
subtitle: "一键SSL"
tags:
     - iOS
     - HTTPS
     - GithubPages
---


> 收到Kloudsec创始人发的推荐邮件,决定试一试 


# 注册
Kloudsec官网地址:[Click!](https://kloudsec.com)  

右上角点击仪表盘,进入demo模式
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/post_imghttps1.png" alt="" class="shadow"/>

添加你的网站
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/post_imghttps2.png" alt="" class="shadow"/>

左侧填写注册信息,右侧点击Register
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/post_imghttps3.png" alt="" class="shadow"/>

邮箱确认激活账号,可能会在垃圾邮件里.

# DNS

登录账号,按照网页提示,将你的域名解析到Kloudsec提供的IP, 也就是添加 A ,和一个 TXT 验证域名拥有权.详细不多说,官网很清楚,相信你也知道如何更改解析.

# SSL
更改好解析,网站会有提醒. 之后点击左侧Protection 进入 我们需要的功能页面. 点击一键开启SSL就行了.等待,很快会有成功邮件.接下来当你访问你的GithubPages 就已经是```https://``` 了

# Done 

> 当然,这一切都是免费的.并且更换解析到Kloudsec提供的IP,也可以加速网站访问.实测ping github服务器ip 140ms, 更换解析之后90多ms.