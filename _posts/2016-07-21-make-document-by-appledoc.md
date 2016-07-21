---
layout: post
title: "使用AppleDoc生成文档"
date: 2016-07-19
author: "Asingers"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-21_2016doc.jpeg"
subtitle: "持续整理"
catalog: true
categories: ios
tags:
   - Mac
   - iOS
   - 开发
   
---


有几种安装方法,这里我推荐用Brew安装:  [Github地址](https://github.com/tomaz/appledoc)

    brew install appledoc   
    
我们需要生成的是外部文档,比如一个html,所以只使用命令:   

    cd 到工程所在目录,我以docTest为例    
    
    需要参数:
    
    // --output 输出目录
    // --project-name
    // --project-company
    // --company-id com.asingers  
    
    appledoc --no-create-docset --output /Users/asingers/Desktop/doc --project-name docTest  --project-company asingers --company-id com.asingers .  
    
    
    
    
   
    
    
    
