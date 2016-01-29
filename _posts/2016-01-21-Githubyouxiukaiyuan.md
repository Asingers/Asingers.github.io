---
layout: post
title:  "Github优秀开源项目大全"
date:   2016-01-21 15:06:00
author:     "Asingers"
comments: true
categories: iOS
tags:
     - Github
     - 开源
---

## 完整客户端

<ul>
<li><a href="https://github.com/dennisreimann/">ioctocat</a></li>
</ul>


<p>github的iOS客户端，目前开源代码是V1版本，V2版本在appstore上可以下载</p>

<ul>
<li><a href="https://github.com/chrisballinger/ChatSecure-iOS">ChatSecure-iOS</a></li>
</ul>


<p>使用XMPP协议的IM开源软件，很强大，在appstore上可以下载</p>

<!-- more -->


<ul>
<li><a href="https://github.com/gaosboy/iOSSF">SegmentFault</a></li>
</ul>


<p>SegmentFault的官方iOS客户端</p>

<ul>
<li><a href="http://git.oschina.net/oschina/iphone-app">OSChina-iOS</a></li>
</ul>


<p>开源中国社区oschina的官方iPhone客户端，appstore已上线。早期地址在<a href="https://github.com/gaosboy/iOSSF">github</a>上,后来迁移到OSChina自己的代码托管平台.</p>

<ul>
<li><a href="https://github.com/fggeraissate/FFCalendar">FFCalendar</a></li>
</ul>


<p>实现了日历的基本功能，目前只支持iPad版本</p>

<p><img src="https://raw.githubusercontent.com/fggeraissate/FFCalendar/master/FFCalendar/FFCalendars/Util/Images/YearlyCalendar.png" width="320" height="480"></p>


<ul>
<li><a href="https://github.com/WhiteHouse/wh-app-ios">wh-app-ios</a></li>
</ul>


<p>美国白宫（WhiteHouse）的官方app，听起来很高大上哈</p>

<ul>
<li><a href="https://github.com/ruby-china/ruby-china-for-ios">ruby-china-for-ios</a></li>
</ul>


<p>Ruby China的官方app</p>

<ul>
<li><a href="https://github.com/nothingmagical/cheddar-ios">cheddar-ios</a></li>
</ul>


<p>一款不错的日程管理软件，Appstore上能下载</p>

<p><img src="https://github.com/wangzz/wangzz.github.com/blob/master/images/cheddar-ios-screen-short.jpeg?raw=true" width="320" height="480"></p>


<ul>
<li><a href="https://github.com/jimpick/twitterfon">twitterfon</a></li>
</ul>


<p>第三方twitter客户端，不过作者上传后至今5年了都没更新过。。。</p>

<ul>
<li><a href="https://github.com/viewfinderco/viewfinder">viewfinder</a></li>
</ul>


<p>移动支付公司Square在其工程博客上宣布，基于Apache 2.0许可协议，开源了于去年12月初收购的照片管理和共享应用Viewfinder，包括Viewfinder服务器、Android和iOS应用在内的25万行代码已托管到GitHub上。
对此，Square工程师Peter Mattis在<a href="http://corner.squareup.com/2014/05/open-sourcing-viewfinder.html">工程博客</a>上表示，Square之所以考虑到将Viewfinder的完整代码公之于众，是希望能够与人方便，让开发者在应用开发过程中可以加以利用或作为参考。尽管Square团队并没有为Viewfinder提供技术支持，也没有进行Bug修复，但此举还是赢得了满堂喝彩一致点赞。</p>

<p>Viewfinder包含了许多非常有趣的代码，对于开发者来说，绝对是大大的Surprise，主要如下：</p>

<pre><code>. Viewfinder服务器提供了一个拥有各种Amazon DynamoDB索引选项的结构化数据库架构。
. 服务器还提供了数据库和协议层版本控制支持。
. 在本地元数据存储方面，Viewfinder客户端使用LevelDB，相比CoreData，更易于使用，也相当便捷。
. 内置可直接运行于移动设备上的全文本搜索引擎，支持联系人和图片搜索。
. 使用GYP生成Xcode项目文件和Android构建文件。
. 支持C++模板元编程，可使用C++11可变参数模板根据C++方法自动计算Java方法签名。
</code></pre>

<p>该段介绍出自<a href="http://www.pcbeta.com/viewnews-63336-1.html">这里</a>。</p>

<p>viewfinder使用GYP生成Xcode的工程文件，生成方式如下：</p>

<p>首先要安装GYP，执行以下步骤：</p>

<figure class='code'><div class="highlight"><table><tr> 
</pre></td><td class='code'><pre><code class=''><span class='line'>$ svn checkout http://gyp.googlecode.com/svn/trunk/ gyp-read-only 
</span><span class='line'>$ cd gyp-read-only 
</span><span class='line'>$ ./setup.py build 
</span><span class='line'>$ sudo ./setup.py install </span></code></pre></td></tr></table></div></figure>


<p>安装成功以后，再进入到clone下来的viewfineder源码目录，执行：</p>

<figure class='code'><div class="highlight"><table><tr> 
</pre></td><td class='code'><pre><code class=''><span class='line'>$ cd viewfinder/clients/ios
</span><span class='line'>$ gyp --depth=. -DOS=ios -Iglobals.gypi ViewfinderGyp.gyp</span></code></pre></td></tr></table></div></figure>


<p>这样就能成功生成Xcode工程文件了，不过需要通过<code>ViewfinderGyp.xcodeproj</code>文件打开工程。</p>

<ul>
<li><a href="https://github.com/Xuzz/newsyc">HackerNews</a></li>
</ul>


<p><code>Hacker News</code>的iPhone客户端</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_app_hack_news.png" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/kesalin/AmericanEnglish">AmericanEnglish</a></li>
</ul>


<p>iOS资深开发者<a href="http://blog.csdn.net/kesalin">罗朝辉</a>做的一款应用，《美式英语》的iPhone版本</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_app_american_english.png" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/xincode9/FormosaWeibo">FormosaWeibo</a></li>
</ul>


<p>使用新浪微博开放平台做的微博客户端，做工略显粗糙，作者也有几个月没更新了。</p>

<ul>
<li><a href="https://github.com/ming1016/RSSRead">RSSRead</a></li>
</ul>


<p>AppStore<a href="https://itunes.apple.com/cn/app/yi-yue-rss-li-xian-xin-wen-yue-du/id850246364?mt=8">上线产品</a>，中文名称<code>已阅</code>。一个iOS设备上的RSS/Atom阅读器，刚成立的项目，还有很多有待完善的地方。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_app_rssread.jpeg" width="320" height="480"></p>

<h2>Xcode插件</h2>

<ul>
<li><a href="https://github.com/kattrali/cocoapods-xcode-plugin">cocoapods-xcode-plugin</a></li>
</ul>


<p>用于在Xcode中管理CocoaPods依赖库</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_plugin_cocoapods_menu.png" width="560" height="390"></p>

<ul>
<li><a href="https://github.com/qfish/XAlign">XAlign</a></li>
</ul>


<p>方便实现代码对其功能，使代码风格统一</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_plugin_align.gif" width="560" height="460"></p>

<ul>
<li><a href="https://github.com/fortinmike/XcodeBoost">XcodeBoost</a></li>
</ul>


<p>一个辅助代码编辑插件。支持高亮选中、批量选中方法和方法名、根据选中的方法批量生成方法声明、高亮正则搜索等功能。</p>

<ul>
<li><a href="https://github.com/johnno1962/injectionforxcode">Injection for Xcode</a></li>
</ul>


<p>一个神奇的Xcode 插件，能让应用在运行的时候做出的小的改变立马体现效果，而不需要重新编译。。。</p>

<ul>
<li><a href="https://github.com/supermarin/Alcatraz">Alcatraz</a></li>
</ul>


<p>以图形化界面管理Xcode插件的插件。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_plugin_alcatraz.png" width="560" height="650"></p>

<ul>
<li><a href="https://github.com/ksuther/KSImageNamed-Xcode">KSImageNamed-Xcode</a></li>
</ul>


<p>当输入<code>[NSImage imageNamed:</code> 或者<code>[UIImage imageNamed:</code>时，会自动补全工程中可用的图片名称，同时能提供选中图片的预览。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_plugin_image_named.gif" width="516" height="220"></p>

<ul>
<li><a href="https://github.com/trawor/XToDo">XToDo</a></li>
</ul>


<p>能以图形界面列表的形式列出代码中添加了<code>TODO</code>,<code>FIXME</code>,<code>???</code>,<code>!!!!</code>标识的项目，方便解决软件中备注的未解决问题。另外，能查找的还不只上述四种标识，用户可以自己添加想支持的标识。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_plugin_xtodo.png" width="516" height="320"></p>

<ul>
<li><a href="https://github.com/macoscope/CodePilot">CodePilot</a></li>
</ul>


<p>快速查找工程中的文件、代码等资源，和Xcode5自带的<code>Open Quickly</code>功能相似。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_plugin_codepilot.png" width="516" height="540"></p>

<ul>
<li><a href="https://github.com/onevcat/VVDocumenter-Xcode">VVDocumenter-Xcode</a></li>
</ul>


<p>提供了为代码增加注视的最快捷方式，是我使用频率最高的插件，<a href="http://onevcat.com/">猫神</a>出品。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_plugin_vvdocumenter.gif" width="516" height="300"></p>

<h2>工具</h2>

<ul>
<li><a href="https://github.com/johnno1962/Xtrace">Xtrace</a></li>
</ul>


<p>能详细打印出一个某个方法被调用的堆栈，方便调试时定位问题</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_tool_xtrace.png" width="560" height="460"></p>

<ul>
<li><a href="https://github.com/realmacsoftware/RMConnecter">RMConnecter</a></li>
</ul>


<p>在上传AppStore时需要填写app的描述信息，此软件能很方便的填写这些信息。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_tool_rnconnecter.png" width="580" height="500"></p>

<ul>
<li><a href="https://github.com/facebook/xctool">xctool</a></li>
</ul>


<p>facebook出的自动编译工具，不像xcodebuild，它能够整洁的打印出日志</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_tool_xctool.gif" width="584" height="414"></p>

<ul>
<li><a href="https://github.com/kstenerud/iOS-Universal-Framework">iOS-Universal-Framework</a></li>
</ul>


<p>用于生成兼容armv6/armv7/i386 <code>framework</code>的Xcode工程模版：</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_tool_framework.png" width="584" height="414"></p>

<ul>
<li><a href="https://github.com/kharrison/UYLPasswordManager">UYLPasswordManager</a></li>
</ul>


<p>对访问iOS Keychain的封装库。</p>

<ul>
<li><a href="https://github.com/sqlcipher/sqlcipher">sqlcipher</a></li>
</ul>


<p>这是目前我已知的唯一一个支持对SQLite加密的免费开源库，对应的有收费版本。本开源库实现了对SQLite开源免费版本中未实现的加密接口，同时做了一定的扩展。使用的是256位AES加密方式。</p>

<p>强烈推荐！</p>

<ul>
<li><a href="https://github.com/mattt/Xcode-Snippets">Xcode-Snippets</a></li>
</ul>


<p><code>AFNetworking</code>作者、mattt大神开源的常用Xcode代码片段。</p>

<h2>开发框架</h2>

<ul>
<li><a href="https://github.com/facebook/pop">pop</a></li>
</ul>


<p>facebook那神奇的动画引擎，你懂得。。。</p>

<p><img src="https://github.com/facebook/pop/blob/master/Images/pop.gif?raw=true" alt="pop" /></p>

<ul>
<li><a href="https://github.com/facebook/KVOController">KVOController</a></li>
</ul>


<p>facebook出品，基于Cocoa的KVO开发，提供简单地使用方式，同时也是线程安全的。</p>

<ul>
<li><a href="https://github.com/steipete/Aspects">Aspects</a></li>
</ul>


<p>通过method swizzling技术，能够在一个类的现有方法执行之前或之后附加一个代码片段（以block方式），能极大的方便我们调试。</p>

<ul>
<li><a href="https://github.com/PSPDFKit/PSPDFKit-Demo">PSPDFKit</a></li>
</ul>


<p>十分强大的PDF开发框架，有异步加载、预览、编辑、加标注等很多功能</p>

<ul>
<li><a href="https://github.com/xhacker/TEAChart">TEAChart</a></li>
</ul>


<p>使用简单，功能强大的图表工具</p>

<p><img src="https://github.com/wangzz/wangzz.github.com/blob/master/images/TEAChart-screen-short.gif?raw=true" alt="TEAChart" /></p>

<ul>
<li><a href="https://github.com/kewenya/SearchCoreTest">SearchCoreTest</a></li>
</ul>


<p>一个联系人搜索库，支持的搜索方式包括：用户名汉字、拼音及模糊搜索，号码搜索，最重要的是支持T9搜索，做过通讯录类应用的同学都懂的。我在项目里用过，很赞。</p>

<ul>
<li><a href="https://github.com/robbiehanson/XMPPFramework">XMPPFramework</a></li>
</ul>


<p>应该是XMPP协议Objective-C实现的最好版本，小型开发者想做IM应用的好选择，使用起来也很方便。</p>

<ul>
<li><a href="https://github.com/jessesquires/JSQMessagesViewController">JSQMessagesViewController</a></li>
</ul>


<p>一个通用聊天界面框架，效果不错，感谢作者的开源。这个框架后来被国内某无耻程序员修改成<a href="https://github.com/xhzengAIB/MessageDisplayKit">MessageDisplayKit</a>，大有据为己有之势。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_framework_JSQMessagesViewController.png" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/hailongz/vTeam">vTeam</a></li>
</ul>


<p>一个开发者积累多年的开发框架，值得看看。</p>

<ul>
<li><a href="https://github.com/hfossli/AGGeometryKit">AGGeometryKit</a></li>
</ul>


<p>几何图形框架，把AGGeometryKit和POP结合起来使用，可实现非常棒的动态和动画。</p>

<ul>
<li><a href="https://github.com/Intermark/IMQuickSearch">IMQuickSearch</a></li>
</ul>


<p>IMQuickSearch是一个快速搜索工具，可以过滤包含多种自定义NSObject类的NSArray。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_framework_IMQuickSearch.gif" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/honcheng/iOSPlot">iOSPlot</a></li>
</ul>


<p>新加坡开发者<code>honcheng</code>实现的图标制作框架，支持折线图、饼状图等。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_framework_iOSPlot.png" width="480" height="320"></p>

<h2>自定义view</h2>

<ul>
<li><a href="https://github.com/yishuiliunian/DZTableView">DZTableView</a></li>
</ul>


<p>仿照UITableView机制自己实现的一个自定义tableview，带有详细的说明文档</p>

<ul>
<li><a href="https://github.com/andreamazz/AMWaveTransition">AMWaveTransition</a></li>
</ul>


<p>很炫的带有表格的视图控制器切换效果，点击每个栏目会有限带有波浪效果的信息展示，类似于Facebook Paper</p>

<p><img src="https://raw.githubusercontent.com/andreamazz/AMWaveTransition/master/screenshot.gif" alt="AMWaveTransition" /></p>

<ul>
<li><a href="https://github.com/facebook/Shimmer">Shimmer</a></li>
</ul>


<p>又是facebook出的，可以让view展示波光粼粼的效果</p>

<p><img src="https://github.com/facebook/Shimmer/blob/master/shimmer.gif?raw=true" alt="Shimmer" /></p>

<ul>
<li><a href="https://github.com/steipete/PSTCollectionView">PSTCollectionView</a></li>
</ul>


<p>仿照系统的UICollectionView的API实现的collection view，支持ARC和iOS4.3+系统，可用于替代只能从iOS6开始支持的UICollectionView</p>

<ul>
<li><a href="https://github.com/jaydee3/JDStatusBarNotification">JDStatusBarNotification</a></li>
</ul>


<p>各种形式在状态栏展示信息，包括提示、进度等，展示格式和动画方式也有好几种。下图只是以静态方式展示其效果，更多详情请点击链接查看。</p>

<p><img src="https://github.com/wangzz/wangzz.github.com/blob/master/images/article1/styles.png?raw=true" alt="JDStatusBarNotification" /></p>

<ul>
<li><a href="https://github.com/heroims/SphereView">SphereView</a></li>
</ul>


<p>一个球形3D标签，能够放大、缩小、拖动、点击、自动旋转。效果挺玄的，就是感觉有点卡，还有一定的优化空间。下图截了一个静态图片:</p>

<p><img src="https://github.com/wangzz/wangzz.github.com/blob/master/images/article1/SphereView.png?raw=true" alt="SphereView" /></p>

<ul>
<li><a href="https://github.com/romaonthego/RESideMenu">RESideMenu</a></li>
</ul>


<p>iOS7风格的侧滑菜单，支持左右双向侧滑：</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_RESideMenu.gif" alt="RESideMenu" /></p>

<ul>
<li><a href="https://github.com/gcamp/GCDiscreetNotificationView">GCDiscreetNotificationView</a></li>
</ul>


<p>一种在view的顶部弹出并会自动消失的通知类view，是toast的一种变形。目前<a href="http://git.oschina.net/oschina/iphone-app">开源中国</a>的项目正在用该view。</p>

<ul>
<li><a href="https://github.com/cleexiang/CLProgressHUD">CLProgressHUD</a></li>
</ul>


<p>大麦网iOS客户端工程师开源的一个HUD view，</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_clprogresshud.gif" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/romaonthego/REMenu">REMenu</a></li>
</ul>


<p>自定义的下拉菜单</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_remenu.gif" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/5sw/SWParallaxScrollView">SWParallaxScrollView</a></li>
</ul>


<p>能够实现在多个图层上以不同速度滑动的自定义ScrollView，可用于做软件启动时的help界面：</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_parallax_scrollview.gif" width="480" height="320"></p>

<ul>
<li><a href="https://github.com/tjeerdintveen/Vurig-Calendar">Vurig-Calendar</a></li>
</ul>


<p>自定义的日历，界面很简洁，月份切换时动画效果也不错。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_framework_Vurig-Calendar.png" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/xiangwangfeng/M80AttributedLabel">M80AttributedLabel</a></li>
</ul>


<p>功能较齐全的attributed lable，支持attributed string和图片、链接、控件的混排。</p>

<ul>
<li><a href="https://github.com/Ciechan/BCMeshTransformView">BCMeshTransformView</a></li>
</ul>


<p>实现了相当炫的拉幕式的界面切换效果，其灵感来自CALayer的私有属性<code>meshTransform</code>以及和其对应的<code>CAMeshTransform</code>。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_BCMeshTransformView.jpg" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/cyndibaby905/TwitterCover">TwitterCover</a></li>
</ul>


<p>新浪微博开发者仿照Twitter的iOS客户端中的效果实现的向下拉动滚动视图，视图顶端的图片会随着下拉而变大，并且带有模糊的效果。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_TwitterCover.gif" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/tristanhimmelman/THContactPicker">THContactPicker</a></li>
</ul>


<p>模仿系统邮件应用实现的联系人选择界面。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_THContactPicker.gif" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/kronik/DKCircleButton">DKCircleButton</a></li>
</ul>


<p>一个扁平化的，能带声波效果的按钮。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_DKCircleButton.gif" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/honcheng/PaperFold-for-iOS">PaperFold-for-iOS</a></li>
</ul>


<p>新加坡开发者<code>honcheng</code>实现的折纸效果的界面切换，适合做电子书阅读类应用。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_PaperFold-for-iOS.png" width="320" height="480"></p>

<ul>
<li><a href="https://github.com/honcheng/RTLabel">RTLabel</a></li>
</ul>


<p>新加坡开发者<code>honcheng</code>多媒体显示view，支持html语法，应用非常广泛。</p>

<p><img src="http://7xoawu.com1.z0.glb.clouddn.com/youxiukaiyuan_view_RTLabel.png" width="320" height="480"></p>
