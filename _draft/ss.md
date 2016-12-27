



**教程正式开始**

<ul>
<a href="http://www.vultr.com/?ref=7071587-3B"><img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-12-27-15%3A29%3A43.jpg" </a>
</ul>  
1. 这里需要申请一台免费的Vultr 主机（1000Mbps经典网络，不限流量），使用本推荐链接注册账户免费领取 $50美刀  **[http://www.vultr.com](http://www.vultr.com/?ref=7071587-3B) ** ****免费送 50G 备份空间，还支持免流****

点击**[http://www.vultr.com](http://www.vultr.com/?ref=7071587-3B)**进入官网，输入邮箱和密码,点击Create Account 注册,

然后到邮箱打开新收到的邮件点击Verify Your E-mail,然后在Vultr点击log in登陆关联信用卡Credit Card(不论是银联还是任何其他的信用卡都可以)或者Paypal。(没有信用卡的朋友也不要灰心,可以注册一个paypal账号(新注册paypal用户有10$美金的新手奖励,如果你有信用卡就可以不用注册paypal,直接在vultr网站上关联信用卡.


将信用卡的姓名拼音(重要)、卡号、有效期、验证码填入相应框中，地址邮编随便写,下拉列表可以选择只绑卡还是同时充值,将最底下 I Agree to the Terms of Service 前面的选框打上勾,最后点击最下方的蓝色条Link Credit Card确认。

或者你也可以注册Paypal,避免信用卡盗刷，万一被盗刷，Paypal的赔付速度也是很快的, 注册地址  [https://www.paypal.com/c2/webapps/mpp/account-selection](https://www.paypal.com/c2/webapps/mpp/account-selection)  注册Paypal时选择个人账户(外币转换手续费不用你支付，由卖家支付)，创建账户并验证邮箱后，在我的账户中上方可以看到账户状态，显示未认证，绑定你的信用卡 /借记卡/储蓄卡进行认证，，Palpay会从你的信用卡 /借记卡/储蓄卡扣除几毛钱左右的费用，这样认证就完成了,认证完成后几毛钱会再退回给你,
认证完打开链接:[https://www.paypal.com/selfhelp/contact/call](https://www.paypal.com/selfhelp/contact/call)打里面的客服电话(客服讲中文),输入自己的动态识别码，跟连线客服表达自己是新账户，希望申请 10$的代金券，一般都会同意，不同意就多打几次，不同意就多打几次，不同意就多打几次(有网友反应客服以活动结束之类的理由不发代金券,客服可能以为你在骗领10美金,并不是用来海外消费, 要是不幸碰上这样的客服,只能先挂断电话,再多打几次换别的客服重新申请,态度好点表明用意基本上没问题,目前已经有好多网友成功申领到10美金了)。然后等10分钟就到Paypal账上了(友情提醒:每人只申领一次就好,不要多次申领小心被封号)

*2.激活Vultr(有20$的代金券*2)*

点击**[http://www.vultr.com](http://www.vultr.com/?ref=7071587-3B)**进入官网，点击Create Account注册后，登陆后左边
Billing 账单方式选择关联信用卡Credit Card或者Paypal (如果选择Paypal激活需要预存10$）。

![static/image/common/none.gif](http://images.weiphone.net/data/attachment/forum/201611/02/224848e57egehgmegv6hz7.png)

这样一共就有免费的50-65$(通过我的推荐链接注册)  
  
回到Vulrt的界面，点击右侧的＋号，deploy一个新的VPS(虚拟服务器)。Server Location选择Tokyo（或Los Angeles），Server type选择64 bit OS 下面的Debian 7 x64 （重要）,Server Size选5$一个月的(你也可以选更高级的套餐,总之价钱越高配置就越高级)其他地方不用管,默认即可。最后点击右下角的Deploy Now生成，之后跳转到管理页面（Servers），当显示绿色的running时，该VPS就部署好了，然后你点击o/s下面的按钮就可以看到VPS的IP和密码信息(密码password请点击“……”右边的眼睛图标查看)

![](http://images.weiphone.net/data/attachment/forum/201609/24/140913xjs003vfa7lisx66.jpg)

【如果您是windows电脑系统】下载PUTTY[http://pan.baidu.com/s/1sl1n6qT](http://pan.baidu.com/s/1sl1n6qT)  (也可网上自己搜索下载),然后复制你的VPS的IP,打开putty在主机/IP栏粘贴你vps的IP地址,右下角点击打开, 会出现一个窗口，接着点击是。然后输入root回车。


复制你的VPS密码然后移动鼠标到putty上粘贴**(粘贴方式为单击鼠标右键一次, 记住只需要单击右键一次，注意:为了密码安全,这里单击完鼠标右键不会显示任何内容，但其实是已经输入了,不要重复单击右键)**接着按回车登陆, 出现{root@vurlt~}。


【如果您是苹果电脑系统】，更简单，无需下载PuTTY，系统可以直接连接VPS。
打开“终端”，输入


1. ssh root@ip


*复制代码*
其中“ip”要替换成你VPS的ip地址,回车，
然后输入密码回车登录就好。
详见：[http://www.cnblogs.com/ghj1976/archive/2013/04/19/3030159.html](http://www.cnblogs.com/ghj1976/archive/2013/04/19/3030159.html)


**然后运行以下三行命令(请把每一行单独复制粘贴到putty并按回车)：**
第一行(请点击下面复制代码按钮)


1. wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh


*复制代码*
**第二行**(请点击下面复制代码按钮)


1. chmod +x shadowsocks.sh


*复制代码*
**第三行**(请点击下面复制代码按钮)


1. ./shadowsocks.sh 2>&1 | tee shadowsocks.log


*复制代码*

期间没出现[{root@vurlt](mailto:{root@vurlt)~}就不要动，这个命令是给服务器装SS。中间会提示你输入你的SS SERVER的密码和端口。建议你自己输入一个密码和端口(端口范围1-65536，推荐10000以上),如果不输入,系统会启用默认密码端口。然后按任意键继续,


稍等一会跑完命令后会出来你的SS客户端的信息,记得记下来：



看到以上提示后就表明VPS上SS已经安装成功，并且已经设置了开机启动，VPS重启后不用手工启动SS。

此时，你的VPS重新启动，服务端已经完全配置完毕，如果putty弹出一个连接已断开的提示框(不是报错)，关闭重新打开putty重新登陆即可。
*别光顾着看教程哦，请不要吝啬你的举手之劳，将本教程转发给身边朋友(公开) ，以帮助更多的人。谢谢！*

**至此，****就安装成功可以科学上网了,剩下的就是下载客户端安装到你的手机和电脑上，记得修改客户端的相关设置保持和你的服务端参数一致哦。****客户端****下载和使用请看:[http://bbs.feng.com/read-htm-tid-10797604.html](http://bbs.feng.com/read-htm-tid-10797604.html)**


**已经科学上网成功且对速度满意的小伙伴可以直接略过下面这部分内容**


锐速加速 安装方法

1.请点击下面复制代码按钮复制到putty里按回车：



1. wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh


*复制代码*
2. 然后再点击下面复制代码按钮复制到putty里按回车)



1. vi /serverspeeder/etc/config


*复制代码*
然后在键盘上按方向键移动光标到要修改的数值上,如果原来的数值和下面给出的一致，则不需要修改(由于列表较长,将光标一直按下去 就能找到rsc和gso ) 再按键盘上字母 i 进入编辑模式,  直接修改底下四个参数数值为1 (按键盘上字母1, 如果按Delete键则为删除) ,编辑好后请按键盘上ESC键退出编辑模式 ,  然后在键盘上按shift键加 : 键, 输入wq 按回车）：

rsc="1"
gso="1"
maxmode="1"
advinacc="1"


3. 重启加速服务完成优化(请点击下面复制代码按钮复制到putty里按回车)：



1. service serverSpeeder restart


*复制代码*


至此，就成功加速,  可以超高速看视频了.锐速加速效果还是很明显的。

*别光顾着看教程哦，请不要吝啬你的举手之劳，将本教程转发给身边朋友(公开) ，以帮助更多的人。谢谢！*

*(更新:1.想要免费搭Surge/ shadowrocket 免流量上网的,可以看我写的另一详细菜鸟教程:*
[http://bbs.feng.com/read-htm-tid-10667573.html](http://bbs.feng.com/read-htm-tid-10667573.html)
**整个教程到这里就结束了，我按照自己写的内容重新搭建了一遍没有任何问题，大家有什么问题请尽量在 原帖子 下面提问，在提问之前请先 确保 你是 严格 按照教程一步步 认真 往下执行了的。最后请不要吝啬你的举手之劳，转发本教程给墙内外的朋友 ，以帮助更多的人。谢谢！**
**如果你觉得教程对你有帮助，请不要吝啬你的举手之劳，****请给我加分并将教程转发给身边朋友(公开) ，以帮助更多的人。谢谢!**
**附效果图 :**
**1 双击打开后如图设置SS服务器的信息,然后点击确定****2.在右下角任务栏SS图标处点击鼠标右键如图设置**

http://www.vultr.com/?ref=7053298
http://www.vultr.com/?ref=7071587-3B


