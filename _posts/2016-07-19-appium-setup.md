---
layout: post
title: "Appium iOS 自动化测试环境搭建 "
date: 2016-07-19
author: "Alpaca"
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

### 编写测试用例  


在测试领域中，系统登录这个功能点的地位，堪比软件开发中的`Hello World`，因此第一个测试用例就毫无悬念地选择系统登录了。

在编写自动化测试脚本之前，我们首先需要清楚用例执行的路径，路径中操作涉及到的控件，以及被操作控件的属性信息。

对于本次演示的APP来说，登录时需要先进入【My Account】页面，然后点击【Login】进入登录页面，接着在登录页面中输入账号密码后再点击【Login】按钮，完成登录操作。

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-19_DJI_Plus_Login.png" alt="" class="shadow"/>
确定了操作路径以后，就可以在`Appium Ruby Console`中依次操作一遍，目的是确保代码能正确地对控件进行操作。

第一步要点击【My Account】按钮，因此先查看下Button控件属性。要是不确定目标控件的类型，可以直接执行`page`命令，然后在返回结果中根据控件名称进行查找。


    [1] pry(main)> page :button
    ...（略）
    UIAButton
       name, label: My Account
       id: My Account=> My Account
    nil



通过返回结果，可以看到【My Account】按钮的name、label属性就是“My Account”，因此可以通过`button_exact('My Account')`方式来定位控件，并进行点击操作。


    [2] pry(main)> button_exact('My Account').click
    nil



执行命令后，观察iOS模拟器中APP的响应情况，看是否成功进入“My Account”页面。

第二步也是类似的，操作代码如下：


    [3] pry(main)> button_exact('Login').click
    nil



进入到登录页面后，再次查看页面中的控件信息：


    [4] pry(main)> page
    ...（略）
    UIATextField
       value: Email Address
       id: Email Address=> Email Address
    UIASecureTextField
       value: Password(6-16 characters)id: Password(6-16 characters)=> Password(6-16 characters)UIAButton
       name, label: Login
       id: Log In=> Login
           登录=> Login
    ...（略）



第三步需要填写账号密码，账号密码的控件属性分别是`UIATextField`和`UIASecureTextField`。由于这两个控件的类型在登录页面都是唯一的，因此可以采用控件的类型来进行定位，然后进行输入操作，代码如下：


    [5] pry(main)> tag('UIATextField').type'leo.lee@dji.com'""[6] pry(main)> tag('UIASecureTextField').type'123456'""



执行完输入命令后，在iOS模拟器中可以看到账号密码输入框都成功输入了内容。

最后第四步点击【Login】按钮，操作上和第二步完全一致。


    [7] pry(main)> button_exact('Login').click
    nil



执行完以上四个步骤后，在iOS模拟器中看到成功完成账号登录操作，这说明我们的执行命令没有问题，可以用于编写自动化测试代码。整合起来，测试脚本就是下面这样。


    button_exact('My Account').clickbutton_exact('Login').clicktag('UIATextField').type'leo.lee@dji.com'tag('UIASecureTextField').type'12345678'button_exact('Login').click



将以上脚本保存为`login.rb`文件。

但当我们直接运行`login.rb`文件时，并不能运行成功。原因很简单，脚本中的`button_exact`、`tag`这些方法并没有定义，我们在文件中也没有引入相关库文件。

在上一篇文章中有介绍过，通过`arc`启动虚拟机时，会从`appium.txt`中读取虚拟机的配置信息。类似的，我们在脚本中执行自动化测试时，也会加载虚拟机，因此同样需要在脚本中指定虚拟机的配置信息，并初始化`Appium Driver`的实例。

初始化代码可以通过`Appium Inspector`生成，基本上为固定模式，我们暂时不用深究。

添加初始化部分的代码后，测试脚本如下所示。


    require'rubygems'require'appium_lib'capabilities={'appium-version'=>'1.0','platformName'=>'iOS','platformVersion'=>'9.3',}server_url="http://0.0.0.0:4723/wd/hub"Appium::Driver.new(caps:capabilities).start_driverAppium.promote_appium_methodsObject# testcase: loginbutton_exact('My Account').clickbutton_exact('Login').clicktag('UIATextField').type'leo.lee@dji.com'tag('UIASecureTextField').type'123456'button_exact('Login').clickdriver_quit



## 优化测试脚本：加入等待机制

如上测试脚本编写好后，在Terminal中运行`ruby login.rb`，就可以执行脚本了。

运行命令后，会看到iOS虚拟机成功启动，接着App成功进行加载，然后自动按照前面设计的路径，执行系统登录流程。

但是，在实际操作过程中，发现有时候运行脚本时会出现找不到控件的异常，异常信息如下所示：


    ➜ ruby login.rb
    /Library/Ruby/Gems/2.0.0/gems/appium_lib-8.0.2/lib/appium_lib/common/helper.rb:218:in`_no_such_element': An element could not be located on the page using the given search parameters. (Selenium::WebDriver::Error::NoSuchElementError)
    	from /Library/Ruby/Gems/2.0.0/gems/appium_lib-8.0.2/lib/appium_lib/ios/helper.rb:578:in `ele_by_json'from /Library/Ruby/Gems/2.0.0/gems/appium_lib-8.0.2/lib/appium_lib/ios/helper.rb:367:in`ele_by_json_visible_exact'
    	from /Library/Ruby/Gems/2.0.0/gems/appium_lib-8.0.2/lib/appium_lib/ios/element/button.rb:41:in `button_exact'from /Library/Ruby/Gems/2.0.0/gems/appium_lib-8.0.2/lib/appium_lib/driver.rb:226:in`rescueinblock(4 levels)inpromote_appium_methods'
    	from /Library/Ruby/Gems/2.0.0/gems/appium_lib-8.0.2/lib/appium_lib/driver.rb:217:in `block (4 levels) in promote_appium_methods'from login.rb:28:in`<main>'



更奇怪的是，这个异常并不是稳定出现的，有时候能正常运行整个用例，但有时在某个步骤就会抛出找不到控件的异常。这是什么原因呢？为什么在`Appium Ruby Console`中单步操作时就不会出现这个问题，但是在执行脚本的时候就会偶尔出现异常呢？

原来，在我们之前的脚本中，两条命令之间并没有间隔时间，有可能前一条命令执行完后，模拟器中的应用还没有完成下一个页面的加载，下一条命令就又开始查找控件，然后由于找不到控件就抛出异常了。

这也是为什么我们在`Appium Ruby Console`中没有出现这样的问题。因为手工输入命令多少会有一些耗时，输入两条命令的间隔时间足够虚拟机中的APP完成下一页面的加载了。

那针对这种情况，我们要怎么修改测试脚本呢？难道要在每一行代码之间都添加休眠（sleep）函数么？

也不用这么麻烦，针对这类情况，`ruby_lib`实现了`wait`机制。将执行命令放入到`wait{}`中后，执行脚本时就会等待该命令执行完成后再去执行下一条命令。当然，等待也不是无休止的，如果等待30秒后还是没有执行完，仍然会抛出异常。

登录流程的测试脚本修改后如下所示（已省略初始化部分的代码）：


    wait{button_exact('My Account').click}wait{button_exact('Login').click}wait{tag('UIATextField').type'leo.lee@dji.com'}wait{tag('UIASecureTextField').type'123456'}wait{button_exact('Login').click}



对脚本添加`wait`机制后，之前出现的找不到控件的异常就不再出现了。

## 优化测试脚本：加入结果检测机制

然而，现在脚本仍然不够完善。

我们在`Appium Ruby Console`中手工执行命令后，都是由人工肉眼确认虚拟机中APP是否成功进入下一个页面，或者返回结果是否正确。

但是在执行自动化测试脚本时，我们不可能一直去盯着模拟器。因此，我们还需要在脚本中加入结果检测机制，通过脚本实现结果正确性的检测。

具体怎么做呢？

原理也很简单，只需要在下一个页面中，寻找一个在前一个页面中没有的控件。

例如，由A页面跳转至B页面，在B页面中会存在“Welcome”的文本控件，但是在A页面中是没有这个“Welcome”文本控件的；那么，我们就可以在脚本中的跳转页面语句之后，加入一条检测“Welcome”文本控件的语句；后续在执行测试脚本的时候，如果页面跳转失败，就会因为找不到控件而抛出异常，我们也能通过这个异常知道测试执行失败了。

当然，对下一页面中的控件进行检测时同样需要加入等待机制的。

登录流程的测试脚本修改后如下所示（已省略初始化部分的代码）：


    wait{button_exact('My Account').click}wait{text_exact'System Settings'}wait{button_exact('Login').click}wait{button_exact'Forget password?'}wait{tag('UIATextField').type'leo.lee@dji.com'}wait{tag('UIASecureTextField').type'12345678'}wait{button_exact('Login').click}wait{text_exact'My Message'}



至此，系统登录流程的自动化测试脚本我们就编写完成了。  


> 内容参考: http://debugtalk.com/  



