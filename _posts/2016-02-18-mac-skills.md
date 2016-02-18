---
layout: post
title: "Mac 技巧"
subtitle: "Mac 技巧合集"
date: 2016-02-18 
author: "Asingers"
categories: life
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/post_imgmacskills.jpeg"
tags:
    - Life
    - Mac
---

Mac的通用快捷键

    这部分内容之前陆续介绍过，但还是有童靴希望有个汇总，基于二八原则，我把最常用的快捷键罗列一下，对于非开发者，应该够用了：
     Command+Tab : 任意情况下切换应用程序 - 向前循环
     Shift+Command+Tab : 切换应用程序 - 向后循环
     Command+Delete : 把选中的资源移到废纸篓
     Shift+Command+Delete : 清倒废纸篓（有确认）
     Shift+Option+Command+Delete : 直接清倒废纸篓
     Command+~ 同一应用程序多窗口间切换
     Command+F : 呼出大部分应用程序的查询功能
     Command+C/V/X : 复制/粘贴/剪切
     Command+N : 新建应用程序窗口
     Command+Q : 退出当前应用程序，说明一下，所有应用程序界面左上角都有红黄绿三个小图标，点击绿色扩展到最适合的窗口大小，黄色最小化，红色关掉当前窗口，但并没有退出程序。用Command+Q配合Command+Tab关闭应用程序最为迅速
     Command+L : 当前程序是浏览器时，可以直接定位到地址栏
     Command+"+/-" 放大或缩小字体
     Control+Space : 呼出Spotlight
     Command+Space : 切换输入法
        
     对于最后两个快捷键，我个人比较习惯Control+Space切换输入法，所以做了自定义的配置。
    
     光标定位：
     option+←或→ ：光标向左 / 右移动一个词
     command+←或→ ：光标定位在该行开头 / 结尾
     command+↑或↓：到达该页首 / 页尾
     fn+delete：删除后面一个字符
     option+delete：刪除前面的一个词
     command+delete：刪除该行处于光标前的所有字符
     fn+←或→ ：页首 / 页尾
     fn+↑或↓：上一页 / 下一页

H2M_LI_HEADER Mac下想知道某个目录下各个文件和子目录各占多少空间，不需要一个一个去查看。打开终端，在该目录下输入：du -sh *，结果是啥，你们试试就知道了。
H2M_LI_HEADER 
使用sips命令批量处理图片。如果你想批量修改一批图片（尺寸、旋转、反转等），但是你有不会或没有PS，怎么办呢？使用sips命令可以高效完成这些功能，例如：

    \#把当前用户图片文件夹下的所有JPG图片宽度缩小为800px，高度按比例缩放
     sips -Z 800 ~/Pictures/*.JPG
    
     \#顺时针旋转90˚
     sips -r 90 ~/Pictures/*.JPG
    
     \#垂直反转
     sips -f vertical ~/Pictures/*.JPG
    
     更多命令可以用sips -h查看


H2M_LI_HEADER 在Finder中打开文件使用鼠标双击或command+O
H2M_LI_HEADER 
当你在使用系统时如果发现出现异常，那么就就该进行日常维护了。

    打开磁盘管理，选中你的系统盘，点击“修复磁盘权限”，对磁盘权限进行检查和修复。完成之后还可以手动执行维护脚本：
     sudo periodic daily
     sudo periodic weekly
     sudo periodic monthly
     也可以一次全部执行：
     sudo periodic daily weekly monthly
     一般执行完这些操作后，你的Mac就会充满活力，继续上路。这些操作可以定期执行。


H2M_LI_HEADER 
截图

    OS X提供了非常方便的截图工具，你可以随时随地截取屏幕画面。
     shift+command+3：全屏幕截图；shift+command+4：通过鼠标选取截图。
     截取的图片默认存放在桌面上，以时间命名。
     系统默认截图格式是png，你可以通过如下命令修改截图文件类型，例如：
     defaults write com.apple.screencapture type -string JPEG


H2M_LI_HEADER 
Alfred

    - 通过find、open、in搜索文件。find是找到文件，open是找到并打开文件，in是在文件中检索，这种检索方式比spotlight更具备针对性
     - 输入>即可直接运行shell命令。比如>bpython，可以直接打开终端并运行bpython的shell。（收费版本）
     - 输入itunes，会出现一个iTunes mini play，打开可以通过alfred控制音乐播放（收费版本）
     - 输入email，后面跟邮件地址，可以直接打开写邮件的界面（收费版本）
     - 使用alt+command+c，可以调出剪贴板，你的复制历史历历在目（收费版本）


H2M_LI_HEADER 
远程拷贝

    OS X提供基于ssh的远程拷贝命令scp，这个命令大部分linux和unix系统都会提供，使用该命令可以非常方便的在两台机器之间安全的复制文件，具体命令：
     scp ./testfile.txt  username@10.10.10.22:/tmp
     回车后会要求你输入username的密码，只会就当前目录下的testfile.txt复制到另一台机器的tmp目录下。
     scp username@10.10.10.22:/tmp/testfile.txt  ./


H2M_LI_HEADER 
history命令。

    打开终端输入history，所有的历史命令都会显示出来，想找某一条执行过的命令，还可以这样：
     history|grep apache
     找到左边的命令编号（例如时1001），在终端输入
     !1001
     就可以执行原来那条命令了。


H2M_LI_HEADER 
Go2Shell，我们通过Finder浏览文件的时候，常常需要在浏览的文件目录中打开终端进行操作，Go2Shell就能自动做到这一点。

    从[App Store](https://itunes.apple.com/us/app/go2shell/id445770608?mt=12)下载，下载完成后从应用程序文件夹把Go2Shell拖到Finder工具栏上，然后随便进入一个目录，点击Go2Shell图标，即可打开终端进入该目录。
    
     Go2Shell支持原生终端、iTerm2和xterm，比我之前用过的>cd to ...app方便。在终端输入open -a Go2Shell --args config即可进入配置界面，选择你喜欢的终端。


H2M_LI_HEADER 
open

    我们之前介绍过如何在finder中浏览文件时进入当前目录的shell界面，那个插件叫做Go2Shell。当然我们也会有在shell下打开当前目录的finder的需求，运行如下命令即可：
    
    open .
    当然open也可以打开其他目录，比如open /Users
    open
    还可以直接打开文件，打开程序，指定程序打开文件，打开网址等等，例如
    open a.txt
    open -a Safari
    open -a TextMate a.txt
    open http://news.sina.com.cn


H2M_LI_HEADER 
locate

    locate是Unix/Linux下的命令工具，基本原理就是通过定期更新系统的文件和文件名并把索引信息放入系统的数据库中，当通过locate查找文件时直接从数据库里那数据。而且locate可以查到spotlight查不到的系统文件。 基本的使用方法非常简单，比如你想找niginx的配置文件在哪，只需输入：
    locate nginx.conf


H2M_LI_HEADER 
退格键木了，PageUP/PageDown/Home/End也木了。别担心，您不是还有delete键和上下左右方向键么？delete相对于退格键，fn+delete可以往前删，fn+上下左右方向键可以实现PageUP/PageDown/Home/End的功能，一个都不能少。

H2M_LI_HEADER 
在Finder中打开某个文件夹下所有子文件夹

    有时候我们希望在Finder中查看某个文件夹下的所有文件和子文件夹，怎么做到呢？把文件切换到列表视图（command+2），把排序方式设置为不排序，这时文件夹左侧会出现一个箭头。按住option键点击文件夹左侧的箭头，你就会发现所有的文件和文件夹都展现在眼前了。注意，如果该文件夹下文件太多，不建议使用，打开会需要很长时间。


H2M_LI_HEADER 
mdfind是一个非常灵活的全局搜索命令，类似Spotlight的命令行模式，可以在任何目录执行文件名、文件内容进行检索，例如：

    mdfind 苹果操作系统    
    \#搜索文件内容或文件名包含苹果操作系统的文件
    mdfind -onlyin ~/Desktop 苹果操作系统
    \#在桌面上搜索文件内容或文件名包含苹果操作系统的文件
    mdfind -count -onlyin ~/Desktop 苹果操作系统
    \#统计搜索到的结果
    mdfind -name 苹果操作系统
    \#搜索文件名包含苹果操作系统的文件


