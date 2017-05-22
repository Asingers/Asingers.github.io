---
layout: post
title: "如何保存 Windows 10「聚焦」功能的精美壁纸"
date: 2016-08-10
author: "Alpaca"
subtitle: "Tips"
catalog: true
categories: life
tags:
   - Life
---
上月底微软正式关闭了 Windows 10 的免费升级通道，对那些已经升级到 Windows 10 的用户来说，不管你是怎么「莫名其妙地」升级上去的，如何微笑着 live with it 才是更应该考虑的事情：比如说，继续更新最新的周年纪念版；又比如说，优雅地用好 Windows 10。

在所有「优雅地使用 Windows 10」的方法中，开启「Windows 聚焦」功能则是最容易上手的方法之一。

## 什么是「Windows 聚焦」

你可以把「Windows 聚焦」这个功能理解为微软为 Windows 10 内置的「锁屏壁纸自动换」功能，开启这个功能后 Windows 10 会在后台定期为你更换锁屏壁纸，同时在锁屏中偶尔为你提供一些功能建议和小贴士（这个功能的触发频率较低）。

而相比于普通的壁纸自动换应用，「Windows 聚焦」有它独有的优势：高质量壁纸、随着使用过程而不断提高的壁纸筛选能力，前者主观上来说也很大程度上依赖于后者。

开启「Windows 聚焦」后，每当锁屏界面换上了一张新壁纸，壁纸右上角都会出现一个小提示来询问你是否喜欢这张壁纸。你的每一次选择都将为「Windows 聚焦」后续筛选壁纸提供参考，从而让锁屏壁纸越来越符合你的口味。**这个过程有点类似于 A/B 测试或者说红心电台**。

附：微软官方的[相关介绍](https://technet.microsoft.com/zh-cn/library/mt621549(v=vs.85).aspx)

## 如何开启「Windows 聚焦」

在桌面空白处单击右键，左键点击进入列表最下方的「个性化」菜单，在弹出的界面中选择左侧标签的「锁屏界面」选项。

点击「锁屏界面」的「背景」选项，选择启用列表中的「Windows 聚焦」即可。


![http://cdn.sspai.com/attachment/thumbnail/2016/08/03/c24386254a6db924640b877a2b00e82b532eb_mw_800_wm_1_wmp_3.jpg](http://cdn.sspai.com/attachment/thumbnail/2016/08/03/c24386254a6db924640b877a2b00e82b532eb_mw_800_wm_1_wmp_3.jpg)

![http://cdn.sspai.com/attachment/thumbnail/2016/08/03/3c250a3c61526610cf532027d5ba6fc7532ec_mw_800_wm_1_wmp_3.jpg](http://cdn.sspai.com/attachment/thumbnail/2016/08/03/3c250a3c61526610cf532027d5ba6fc7532ec_mw_800_wm_1_wmp_3.jpg)


## 进阶：如何批量保存「Windows 聚焦」壁纸

如上面所说的那样，开启「Windows 聚焦」之后，系统会根据你的反馈向你推荐越来越合你口味的锁屏壁纸。这自然会引发出一系列需求：怎么将「Windows 聚焦」的锁屏壁纸设置为桌面壁纸？既然壁纸对自己胃口，为什么不能把它们永久地保存下来？

诸如此类的需求，都可以通过导出「Windows 聚焦」的壁纸来实现：

1.「Windows 聚焦」的壁纸都保存在一个叫做 Assets 的隐藏文件夹里，打开资源管理器，在地址栏键入下方路径后按回车键，即可快速跳转至这个隐藏的文件夹。为了方便日后访问，建议大家通过左上角功能区将这个文件夹目录固定到资源管理器的「快速访问」区域。

> 
> *%localappdata%\Packages\Microsoft.Windows.ContentDeliveryManager_cw5n1h2txyewy\LocalState\Assets*
> 


2. Assets 文件夹中的各式以乱码命名文件其实就是「Windows 聚焦」下载的壁纸。为了方便查看和导出，这里又建议大家将 Assets 文件夹的查看方式更改为「详细信息」，然后点击「修改日期」对文件夹中的内容进行排序。

3. 为了不影响「Windows 聚焦」功能的正常工作，**请不要修改任何 /Assets 文件夹中的内容**。选择好要导出的壁纸，然后将它们复制到用于保存壁纸的目标文件夹中（如图中的「Focus Collection」）即可。


![http://cdn.sspai.com/attachment/thumbnail/2016/08/03/d0615bdefee1cbad8bd8e47a814257ea532ef_mw_800_wm_1_wmp_3.jpg](http://cdn.sspai.com/attachment/thumbnail/2016/08/03/d0615bdefee1cbad8bd8e47a814257ea532ef_mw_800_wm_1_wmp_3.jpg)

![http://cdn.sspai.com/attachment/thumbnail/2016/08/03/293f4c9c5da94e27ecc3b53b6a89fb40532f0_mw_800_wm_1_wmp_3.jpg](http://cdn.sspai.com/attachment/thumbnail/2016/08/03/293f4c9c5da94e27ecc3b53b6a89fb40532f0_mw_800_wm_1_wmp_3.jpg)


4. 复制完成后，按住「Shift」键然后右键单击用于保存壁纸的文件夹，选择「在此处打开命令窗口」，在命令窗口中输入`ren *.* *.jpg`并回车，这些以乱码命名的图片就「原形毕露」啦。


![http://cdn.sspai.com/attachment/thumbnail/2016/08/03/0a7582377d8d0e9061e0363f4a82a169532f1_mw_800_wm_1_wmp_3.jpg](http://cdn.sspai.com/attachment/thumbnail/2016/08/03/0a7582377d8d0e9061e0363f4a82a169532f1_mw_800_wm_1_wmp_3.jpg)

![http://cdn.sspai.com/attachment/thumbnail/2016/08/03/d6f967859a2ec3f9a1af25bcec6c2816532f2_mw_800_wm_1_wmp_3.jpg](http://cdn.sspai.com/attachment/thumbnail/2016/08/03/d6f967859a2ec3f9a1af25bcec6c2816532f2_mw_800_wm_1_wmp_3.jpg)


5. 接下来你就可以根据需要对这些壁纸进行分类筛选和整理。

值得一提的是，「Windows 聚焦」所提供的壁纸相当良心，不仅分辨率可观，所有壁纸还都提供了横竖两种版式。这样一来不仅桌面壁纸不愁了，手机壁纸也可以跟着桌面来个「配套」，赶紧去试试吧！
