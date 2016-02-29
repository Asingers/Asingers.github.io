---
layout: post
title: "升级Xcode7插件无效的解决方法"
subtitle: "The ways to solve Xcode7 upgrade the plugin is invalid"
date: 2016-02-29 
author: "Asingers"
categories: ios
tags:
    - iOS
    - Dev
---
以VVDocumenter-Xcode为例:

<p>从Xcode 5开始，苹果要求加入UUID证书从而保证插件的稳定性。因此Xcode版本更新之后需要在VVDocumenter-Xcode的Info.plist文件中添加Xcode的UUID。</p>

<h2>步骤如下：</h2>

<h3>一、查看Xcode的UUID</h3>

<h4>方式1</h4>

<p>在终端执行</p>

<pre><code>defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID
</code></pre>

<img src="http://images.90159.com/11/vvdocumenter3.jpg" alt="" class="shadow"/>


<p>拷贝选中的字符串。</p>

<h4>方式2</h4>

<p>在/Applications目录中找到Xcode.app，右键”显示包内容”，进入Contents文件夹，双击Info.plist打开，找到DVTPlugInCompatibilityUUID，拷贝后面的字符串。</p>

<h3>二、添加Xcode的UUID到VVDocumenter-Xcode的Info.plist文件</h3>

<h4>方式1&ndash;插件已经安装完成</h4>

<p>1、打开xcode插件所在的目录：~/Library/Application Support/Developer/Shared/Xcode/Plug-ins；</p>

<p>2、选择已经安装的插件例如VVDocumenter-Xcode，右键”显示包内容”；</p>

<p>3、找到info.plist 文件，找到DVTPlugInCompatibilityUUIDs的项目，添加一个Item，Value的值为之前Xcode的UUID，保存。</p>

<p><img src="http://images.90159.com/11/vvdocumenter4.jpg" alt="VVDocumenter1" /></p>

<h4>方式2&ndash;插件还未安装/重新安装</h4>

<p>1、从GitHub克隆仓库到本地，在Xcode中打开项目，选择项目名称，在TAGETS下选中VVDocumenter-Xcode；</p>

<p>2、选择Info，找到DVTPlugInCompatibilityUUIDs的项目，添加一个Item，Value的值为之前Xcode的UUID；</p>

<p>3、Build项目，VVDocumenter-Xcode会自动安装。</p>

<h3>三、重启Xcode</h3>

<p>Xcode 6之后，重启Xcode时会提示“Load bundle”、 “Skip Bundle”，这里必须选择“Load bundle”，不然插件无法使用。</p>
