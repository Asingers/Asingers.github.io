---
layout: post
title: "Appium iOS 自动化测试环境搭建 "
date: 2016-07-19
author: "Asingers"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
   - Mac
   - 测试
   
---

#### 安装Brew
已安装请忽略  

	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"  

#### 安装Node
已安装请忽略  

	brew install node  


#### 安装appium-doctor  
appium-doctor是一个用于验证appium安装环境的工具，可以诊断出Node/iOS/Android环境配置方面的常见问题。  

	npm install appium-doctor -g  
	
安装完毕后，执行appium-doctor命令即可对Appium的环境依赖情况进行检测；指定--ios时只针对iOS环境配置进行检测，指定--android参数时只针对Android环境配置进行检测，若不指定则同时对iOS和Android环境进行检测。  

	$ appium-doctor --ios                                                                                                                               
	info AppiumDoctor ### Diagnostic starting ###
	info AppiumDoctor  ✔ Xcode is installed at: /Applications/Xcode.app/Contents/Developer
	info AppiumDoctor  ✔ Xcode Command Line Tools are installed.
	info AppiumDoctor  ✔ DevToolsSecurity is enabled.
	info AppiumDoctor  ✔ The Authorization DB is set up properly.
	info AppiumDoctor  ✔ The Node.js binary was found at: /usr/local/bin/node
	info AppiumDoctor  ✔ HOME is set to: /Users/Leo
	info AppiumDoctor ### Diagnostic completed, no fix needed. ###
	info AppiumDoctor
	info AppiumDoctor Everything looks good, bye!
	info AppiumDoctor
	
#### 安装Appium.app  

[官网](http://appium.io/)下载安装  

#### 上手调试 

#### 安装ideviceinstaller备用


	brew install ideviceinstaller  
	
#### 配置
针对模拟器而言  
1. x.app 路径  
2. 相对应的设备名  


如图:
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-19_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-19%20%E4%B8%8A%E5%8D%8811.04.16.png" alt="" class="shadow"/>  

针对真机而言:  
1. 运行demo到设备  
2. 打开设置-开发者-Enable Automation  
3. BundleID
4. 相对应设备名  
5. UUID  

如图:
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-19_%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-07-19%20%E4%B8%8A%E5%8D%8811.10.23.png" alt="" class="shadow"/>

#### 启动 

运行Appium Server，并启动【Inspector】后，整体界面如下图所示.  
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-19_Appium_inspector_introduction.png" alt="" class="shadow"/>  

在右边部分，是启动的模拟器，里面运行着我们的待测APP。我们可以像在真机中一样，在模拟器中执行任意功能的操作，当然，模拟器跟真机毕竟还是有区别的，跟传感器相关的功能，例如摄像头、重力感应等，是没法实现的。

在左边部分，就是`Appium Inspector`。Inspector主要由如下四个部分组成：

- 预览界面区：显示画面与模拟器界面一致；不过，当我们在模拟器中切换界面后，Inspector的预览区中显示图像并不会自动同步，若要同步，需要点击【Refresh】按钮，然后Inspector会将模拟器当前UI信息dump后显示到预览区；在预览区中，可以点击选择任意UI控件。
- UI信息展示区：展示当前界面预览区中所有UI元素的层级关系和UI元素的详细信息；在预览区中点击选择任意UI控件后，在“Details”信息框中展示选中控件的详细信息，包括name、label、value、xpath等属性值；通过层级关系，我们也能了解选中控件在当前界面树状结构中所处的具体位置。
- 交互操作区：模拟用户在设备上的操作，例如单击（tap）、滑动（swipe）、晃动（shake）、输入（input）等；操作动作是针对预览界面区选中的控件，因此在操作之前，务必需要先在预览区点击选择UI元素。
- 脚本生成区：将用户行为转换为脚本代码；点击【Record】按钮后，会弹出代码区域；在交互操作区进行操作后，就会实时生成对应的脚本代码；代码语言可通过下拉框进行选择，当前支持的语言类型有：C#、Ruby、Objective-C、Java、node.js、Python。


在实践操作中，Inspector最大的用途就是在可以可视化地查看UI元素信息，并且可以将操作转换为脚本代码，这对初学者尤为有用。

例如，在预览区点击选中按钮“BUY NOW”，然后在UI信息展示区的Details窗口就可以看到该按钮的所有属性信息。在交互操作区点击【Tap】按钮后，就会模拟用户点击“BUY NOW”按钮，并且在脚本区域生成当次按钮点击的脚本（选择Ruby语言）：


    find_element(:name,"BUY NOW >").click



如上就是使用`Appium Inspector`的一般性流程。

### Appium Ruby Console

有了`Appium Inspector`，为什么还需要`Appium Ruby Console`呢？

其实，`Appium Ruby Console`也并不是必须的。经过与多个熟悉`Appium`的前辈交流，他们也从未用过`Appium Ruby Console`，这说明`Appium Ruby Console`并不是必须的，没有它也不会影响我们对`Appium`的使用。

但是，这并不意味着`Appium Ruby Console`是多余的。经过这些天对`Appium`的摸索，我越发地喜欢上`Appium Ruby Console`，并且使用的频率越来越高，现在已基本上很少使用`Appium Inspector`了。这种感觉怎么说呢？`Inspector`相比于`Ruby Conosle`，就像是`GUI`相比于`Linux Terminal`，大家应该能体会了吧。

`Appium Inspector`的功能是很齐全，GUI操作也很方便，但是，最大的问题就是使用的时候非常慢，在预览界面区切换一个页面常常需要好几秒，甚至数十秒，这是很难让人接受的。

在上一节中也说到了，Inspector最大的用途就是在可以可视化地查看UI元素信息，并且可以将操作转换为脚本代码。但是当我们对`Appium`的常用API熟悉以后，我们就不再需要由工具来生成脚本，因为自己直接写会更快，前提是我们能知道目标控件的属性信息（type、name、label、value）。

在这种情况下，如果能有一种方式可以供我们快速查看当前屏幕的控件属性信息，那该有多好。

庆幸的是，在阅读`Appium`官方文档时，发现`Appium`的确是支持命令行方式的，这就是`Appium Ruby Console`。

`Appium Ruby Console`是采用Ruby语言开发的，在使用方式上面和Ruby的`irb`很类似。

在使用`Appium Ruby Console`时，虚拟机的配置信息并不会从GUI中读取，而是要通过配置文件进行指定。

配置文件的名称统一要求为`appium.txt`，内容形式如下所示：


    [caps]
    platformName = "ios"
    platformVersion = '9.3',
    app = "/path/to/UICatalog.app.zip"
    deviceName = "iPhone Simulator"



其中，`platformName`指定虚拟机操作系统类型，“ios”或者”android”；`platformVersion`指定操作系统的版本，例如iOS的’9.3’，或者Android的’5.1’；`app`指定被测应用安装包的路径。这三个参数是必须的，与Inspector中的配置也能对应上。

在使用`Appium Ruby Console`时，首先需要启动`Appium Server`，通过`GUI`或者`Terminal`均可。

然后，在Terminal中，进入到`appium.txt`文件所在的目录，执行`arc`命令即可启动`Appium Ruby Console`。`arc`，即是appium ruby console首字母的组合。


    ➜ ls
    appium.txt
    ➜ arc[1] pry(main)>



接下来，就可以通过执行命令查询当前设备屏幕中的控件信息。

使用频率最高的一个命令是`page`，通过这个命令可以查看到当前屏幕中所有控件的基本信息。

例如，当屏幕停留在前面截图中的页面时，执行`page`命令可以得到如下内容。


    [1] pry(main)> page
    UIANavigationBar
       name: HomeView
       id: Home=> Home
           米=> m
           去看看=> View
    UIAButton
       name, label: tabbar category gray
    UIAImage
       name: dji_logo.png
    UIAButton
       name, label: tabbar cart gray
    UIATableView
       value: rows 1 to 4 of 15
    UIAPageIndicator
       value: page 2 of 2
    UIATableCell
       name: For the firsttimeeverina hand held camera, the Osmo brings professional, realtime cinema-quality stabilization.
       id: 米=> m
    UIAStaticText
       name, label, value: For the firsttimeeverina hand held camera, the Osmo brings professional, realtime cinema-quality stabilization.
       id: 米=> m
    UIAStaticText
       name, label, value: OSMO
    UIAButton
       name, label: SHOP NOW >
    UIATableCell
       name: Ronin
    UIAStaticText
       name, label, value: Ronin
    UIAStaticText
       name, label, value: Phantom
       id: 米=> m
    ...(略)UIAButton
       name, label: Store
       value: 1
       id: 门店=> Store
    ...(略)UIAButton
       name, label: My Account
       id: My Account=> My Account
    nil



通过返回信息，我们就可以看到所有控件的type、name、label、value属性值。如果在某个控件下没有显示label或value，这是因为这个值为空，我们可以不予理会。

由于`page`返回的信息太多，可能不便于查看，因此在使用`page`命令时，也可以指定控件的类型，相当于对当前屏幕的控件进行筛选，只返回指定类型的控件信息。

指定控件类型时，可以通过string类型进行指定（如 page “Image”），也可通过symbol类型进行指定（如 page :cell）。指定的类型可只填写部分内容，并且不分区大小写。


    [2] pry(main)> page"Image"UIAImage
       name: dji_logo.png
    nil[3] pry(main)> page :cell
    UIATableCell
       name: DJI’s smartest flying camera ever.
       id: 米=> m
    UIATableCell
       name: Ronin
    UIATableCell
       name: Phantom
       id: 米=> m
    nil



如果需要查看当前屏幕的所有控件类型，可以执行`page_class`命令进行查看。


    [4] pry(main)> page_class
    14x UIAButton
    8x UIAStaticText
    4x UIAElement
    4x UIATableCell
    2x UIAImage
    2x UIAWindow
    1x UIAPageIndicator
    1x UIATableView
    1x UIAStatusBar
    1x UIANavigationBar
    1x UIATabBar
    1x UIAApplication
    nil



基本上，`page`返回的控件信息已经足够满足绝大多数场景需求，但有时候情况比较特殊，需要`enabled`、`xpath`、`visible`、坐标等属性信息，这时就可以通过执行`source`命令。执行`source`命令后，就可以返回当前屏幕中所有控件的所有信息，以xml格式进行展现。

## 定位UI控件

获取到UI控件的属性信息后，就可以对控件进行定位了。

首先介绍下最通用的定位方式，`find`。通过`find`命令，可以实现在控件的诸多属性值（`name`、`label`、`value`、`hint`）中查找目标值。查询时不区分大小写，如果匹配结果有多个，则只返回第一个结果。


    [5] pry(main)> find('osmo')#<Selenium::WebDriver::Element:0x..febd52a30dcdfea32 id="2">[6] pry(main)> find('osmo').label"Osmo"



另一个通用的定位方式是`find_element`，它也可以实现对所有控件进行查找，但是相对于`find`，可以对属性类型进行指定。


    [7] pry(main)> find_element(:class_name,'UIATextField')#<Selenium::WebDriver::Element:0x31d87e3848df8804 id="3">[8] pry(main)> find_element(:class_name,'UIATextField').value"Email Address"



不过在实践中发现，采用`find`、`find_element`这类通用的定位方式并不好用，因为定位结果经常不是我们期望的。

经过反复摸索，我推荐根据目标控件的类型，选择对应的定位方式。总结起来，主要有以下三种方式。

针对Button类型的控件（UIAButton），采用`button_exact`进行定位：


    [9] pry(main)> button_exact('Login')#<Selenium::WebDriver::Element:0x..feaebd8302b6d77cc id="4">



针对Text类型的控件（UIAStaticText），采用`text_exact`进行定位：


    [10] pry(main)> text_exact('Phantom')#<Selenium::WebDriver::Element:0x1347e89100fdcee2 id="5">



针对控件类型进行定位时，采用`tag`；如下方式等价于`find_element(:class_name, 'UIASecureTextField')`。


    [11] pry(main)> tag('UIASecureTextField')#<Selenium::WebDriver::Element:0x..fc6f5efd05a82cdca id="6">



基本上，这三种方式就已经足够应付绝大多数测试场景了。当然，这三种方式只是我个人经过实践后选择的定位方式，除了这三种，`Appium`还支持很多种其它定位方式，大家可自行查看`Appium`官方文档进行选择。

另外，除了对控件进行定位，有时候我们还想判断当前屏幕中是否存在某个控件（通常用于结果检测判断），这要怎么做呢？

一种方式是借助于`Appium`的控件查找机制，即找不到控件时会抛出异常（`Selenium::WebDriver::Error::NoSuchElementError`）；反过来，当查找某个控件抛出异常时，则说明当前屏幕中不存在该控件。


    [12] pry(main)> button_exact('Login_invalid')Selenium::WebDriver::Error::NoSuchElementError: An element could not be located on the page using the given search parameters.
    from /Library/Ruby/Gems/2.0.0/gems/appium_lib-8.0.2/lib/appium_lib/common/helper.rb:218:in`_no_such_element'



该种方式可行，但比较暴力，基本上不会采用这种方式。

另一种更好的方式是，查找当前屏幕中指定控件的个数，若个数不为零，则说明控件存在。具体操作上，将`button_exact`替换为`buttons_exact`，将`text_exact`替换为`texts_exact`。


    [12] pry(main)> buttons_exact('Login').count
    1[13] pry(main)> buttons_exact('Login_invalid').count
    0



除此之外，基于Ruby实现的`appium_lib`还支持`exists`方法，可直接返回Boolean值。


    [14] pry(main)> exists{button_exact('Login')}true[15] pry(main)> exists{button_exact('Login_invalid')}false



## 对控件执行操作

定位到具体的控件后，操作就比较容易了。

操作类型不多，最常用就是点击（click）和输入（type），这两个操作能覆盖80%以上的场景。

对于点击操作，才定位到的控件后面添加`.click`方法；对于输入操作，在定位到的输入框控件后面添加`.type`方法，并传入输入值。

例如，账号登录操作就包含输入和点击两种操作类型。


