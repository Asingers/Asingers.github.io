---
layout: post
title: "10分钟搞定支付宝和微信支付 的 各种填坑"
date: 2016-05-09
author: "Asingers"
catalog: true
categories: ios
tags:
   - iOS
---
填坑
支付宝填坑是每个接入支付宝必经之路，下面是我接入支付宝遇到的问题汇总，希望大家在接入的路上少一点弯路

#### 问题1. Util/base64.h:63:21: Cannot find interface declaration for ‘NSObject’, superclass of ‘Base64’

    解决办法：
    这是base64.h中没有加入#import <Foundation/Foundation.h> 系统库文件导致，这个错误报错方法直接想喷它一脸。报错方式太恶心。


#### 问题2.截图告知你什么问题


<img src="http://upload-images.jianshu.io/upload_images/616981-d6540c725f3801a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="" class="shadow"/>




    解决办法：
    这个问题可以同上的，心情好，截图再次说明下，在 openssl_wrapper.h中#import <Foundation/Foundation.h> 库即可


#### 问题3.Util/openssl_wrapper.m:11:9: ‘rsa.h’ file not found

    解决办法：
    （1），万年老坑，只要你接入支付宝百分百要遇到的问题，所以习以为常吧
    （2），在Build setting中搜索search，找到Header Search Paths，添加$(PROJECT_DIR)/openssl和$(PROJECT_DIR) 如下图：
    （3），重要 问题说三遍，这是网络找到的到答案后继续有同样的坑，自己的解决方案,
    Header Search Paths   $(PROJECT_DIR)/ali中输入这个
    Framework Search Paths  和 Library Search Paths 继续是$(inherited)  和  $(PROJECT_DIR)/ali
    ‘rsa.h’ file not found  的解决方案
    （4），由于后期多项目的接入，让我知道一个算是万能方法吧，就是始终保持Header Search Paths 和 Library Search Paths 都能找到你导入的openssl的正确路径即可，已尝试多遍，是能解决以上问题（求黑）


<img src="http://upload-images.jianshu.io/upload_images/616981-b71f879b0c85fb6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="" class="shadow"/>


<img src="http://upload-images.jianshu.io/upload_images/616981-e833ce4b6bd3cb57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="" class="shadow"/>




#### 问题4.这类错很多，大概有这些：这些可能是库文件没有导入，导致的


```
“_CNCopyCurrentNetworkInfo”, referenced from:
Undefined symbols for architecture x86_64:
"*CNCopyCurrentNetworkInfo", referenced from:
-[APayReachability wifiInterface] in AlipaySDK
+[internal_DeviceInfo getSSIDInfo] in AlipaySDK
+[internal_DeviceInfo getNetworkInfo] in AlipaySDK
"_CNCopySupportedInterfaces", referenced from:
-[APayReachability wifiInterface] in AlipaySDK
+[internal_DeviceInfo getSSIDInfo] in AlipaySDK
+[internal_DeviceInfo getNetworkInfo] in AlipaySDK
"_CTRadioAccessTechnologyCDMA1x", referenced from:
-[AliSecXReachability networkStatusForFlags:] in AlipaySDK
"_CTRadioAccessTechnologyEdge", referenced from:
-[AliSecXReachability networkStatusForFlags:] in AlipaySDK
"_CTRadioAccessTechnologyGPRS", referenced from:
-[AliSecXReachability networkStatusForFlags:] in AlipaySDK
"_CTRadioAccessTechnologyLTE", referenced from:
-[AliSecXReachability networkStatusForFlags:] in AlipaySDK
"_OBJC_CLASS*$_CMMotionManager", referenced from:
objc-class-ref in AlipaySDK
"*OBJC_CLASS*$_CTTelephonyNetworkInfo", referenced from:
objc-class-ref in AlipaySDK
"*SCNetworkReachabilityCreateWithAddress", referenced from:
+[APayReachability reachabilityWithAddress:] in AlipaySDK
+[AliSecXReachability reachabilityWithAddress:] in AlipaySDK
"_SCNetworkReachabilityCreateWithName", referenced from:
+[APayReachability reachabilityWithHostname:] in AlipaySDK
+[AliSecXReachability reachabilityWithHostName:] in AlipaySDK
"_SCNetworkReachabilityGetFlags", referenced from:
-[APayReachability isReachable] in AlipaySDK
-[APayReachability isReachableViaWWAN] in AlipaySDK
-[APayReachability isReachableViaWiFi] in AlipaySDK
-[APayReachability connectionRequired] in AlipaySDK
-[APayReachability isConnectionOnDemand] in AlipaySDK
-[APayReachability isInterventionRequired] in AlipaySDK
-[APayReachability reachabilityFlags] in AlipaySDK
...
"_SCNetworkReachabilityScheduleWithRunLoop", referenced from:
-[AliSecXReachability startNotifier] in AlipaySDK
"_SCNetworkReachabilitySetCallback", referenced from:
-[APayReachability startNotifier] in AlipaySDK
-[APayReachability stopNotifier] in AlipaySDK
-[AliSecXReachability startNotifier] in AlipaySDK
"_SCNetworkReachabilitySetDispatchQueue", referenced from:
-[APayReachability startNotifier] in AlipaySDK
-[APayReachability stopNotifier] in AlipaySDK
"_SCNetworkReachabilityUnscheduleFromRunLoop", referenced from:
-[AliSecXReachability stopNotifier] in AlipaySDK
"std::**1::basic_string<char, std::**1::char_traits<char>, std::**1::allocator<char> >::**init(char const*, unsigned long)", referenced from:
CAliSecXURL::encodeURIComponent(CAliSecXBuffer&) in AlipaySDK
"std::**1::basic_string<char, std::**1::char_traits<char>, std::**1::allocator<char> >::reserve(unsigned long)", referenced from:
CAliSecXURL::encodeURIComponent(CAliSecXBuffer&) in AlipaySDK
"std::**1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >::~basic_string()", referenced from:
CAliSecXURL::encodeURIComponent(CAliSecXBuffer&) in AlipaySDK
"std::nothrow", referenced from:
CAliSecXBuffer::CAliSecXBuffer(unsigned long) in AlipaySDK
CAliSecXBuffer::_copy(unsigned char const*, unsigned long) in AlipaySDK
CAliSecXBuffer::resize(unsigned long) in AlipaySDK
"std::terminate()", referenced from:
***clang_call_terminate in AlipaySDK
"operator delete", referenced from:
CAliSecXBuffer::~CAliSecXBuffer() in AlipaySDK
CAliSecXBuffer::release() in AlipaySDK
CAliSecXBuffer::~CAliSecXBuffer() in AlipaySDK
CAliSecXBuffer::operator=(CAliSecXBuffer const&) in AlipaySDK
CAliSecXBuffer::resize(unsigned long) in AlipaySDK
alisec_crypto_Hex2Bin(CAliSecXBuffer const&) in AlipaySDK
alisec_crypto_Bin2Hex(CAliSecXBuffer const&) in AlipaySDK
...
"operator new", referenced from:
CAliSecXBuffer::CAliSecXBuffer(unsigned long) in AlipaySDK
CAliSecXBuffer::_copy(unsigned char const*, unsigned long) in AlipaySDK
CAliSecXBuffer::resize(unsigned long) in AlipaySDK
"***cxa_begin_catch", referenced from:
***clang_call_terminate in AlipaySDK
"***gxx_personality_v0", referenced from:
+[ASSStorageAccesser saveStorageModel:] in AlipaySDK
+[ASSStorageAccesser loadStorageModelFromKeychain] in AlipaySDK
+[ASSStorageAccesser loadPreviousApdid] in AlipaySDK
+[ASSStorageAccesser getRandomizedID] in AlipaySDK
+[ASSStorageAccesser getNewRadomizedID] in AlipaySDK
+[ASSStorageAccesser loadLastLoginTime] in AlipaySDK
+[ASSStorageAccesser saveCurrentLoginTime:] in AlipaySDK
...
"_deflate", referenced from:
+[ASSCommonUtils gzipData:] in AlipaySDK
+[DTGZipUtil compressGZip:] in AlipaySDK
"_deflateEnd", referenced from:
+[ASSCommonUtils gzipData:] in AlipaySDK
+[DTGZipUtil compressGZip:] in AlipaySDK
"_deflateInit2*", referenced from:
+[ASSCommonUtils gzipData:] in AlipaySDK
+[DTGZipUtil compressGZip:] in AlipaySDK
"_kCNNetworkInfoKeyBSSID", referenced from:
+[UIDevice(APEX) networkDic] in AlipaySDK
"_kCNNetworkInfoKeySSID", referenced from:
+[UIDevice(APEX) networkDic] in AlipaySDK
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```
    解决办法：
    这种问题通过在General->Link Framework and Libraiies中添加以下framework解决：
    
    - libz.tbd
    - libc++.tbd
    - Security.framework
    - CoreMotion.Framework
    - CFNetwork.framework
    - CoreTelephony.framework
    - SystemConfiguration.framework


截图如下，由于公司同时接入支付宝和微信支付，所以导入的库就多了点咯：

<img src="http://upload-images.jianshu.io/upload_images/616981-2e9fde123b91a6d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="" class="shadow"/> 



#### 问题5.Redefinition of 'RSA' as different kind of symbol  多为sdk集成时产生的坑，因为我们公司在集成支付宝之前，有用过RSA加密，导致重名问题

    解决办法：
    （1），这个问题不是每个公司都可能遇到的，但遇到也心烦
    （2），由于支付宝中的openssl中的rsa.h文件与RSA加密有重名冲突。改掉公司自己之前导入RSA的命名，如果你牛逼也可以去改rsa.h中的


#### 问题6；系统库导入问题
+++++++++++++
symbol(s) not found for architecture arm64


<img src="http://upload-images.jianshu.io/upload_images/616981-52c8b3ed04cf400f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="" class="shadow"/> 

解决办法
就是导入系统库了


<img src="http://upload-images.jianshu.io/upload_images/616981-3a750ed6a81bdc43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" alt="" class="shadow"/> 


#### 问题7：终于到微信了，接入微信，你就开心了
因为问题太少了，只能感谢下这两个帖子的楼主了
解决办法：  

[使用微信支付SDK1.5版本的支付demo](http://www.cocoachina.com/bbs/read.php?tid-309177-page-1.html)  

[微信支付如果遇到跳转只有一个确定请看这里](http://www.cocoachina.com/bbs/read.php?tid-321546.html)

为了一些懒人懒的去看帖子，简单说，就是微信支付注册放在友盟分享之后就ok了！
代码示例：

    // 友盟分享
        [self configUmengShare];
    //向微信注册
        [WXApi registerApp:@"wxb4ba3c02aa476ea1" withDescription:@"demo 2.0"];
        
        
> 原文来自:[Migi000](http://www.jianshu.com/p/6d67cfe0f00c?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
