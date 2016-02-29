---
layout: post
title: "iOS开发之如何跳到系统设置里的各种设置界面"
subtitle: "How to jump to the various Settings in the system Settings"
date: 2016-02-29 
author: "Asingers"
categories: ios
tags:
    - iOS
    - Dev
---
## 定位服务

定位服务有很多APP都有，如果用户关闭了定位，那么，我们在APP里面可以提示用户打开定位服务。点击到设置界面设置，直接跳到定位服务设置界面。代码如下：

    //定位服务设置界面
    NSURL *url = [NSURL URLWithString:@"prefs:root=LOCATION_SERVICES"];
    if ([[UIApplication sharedApplication] canOpenURL:url])
    {
        [[UIApplication sharedApplication] openURL:url];
    }


这样就可以跳到系统设置的定位服务界面啦！我们继续看几个列子。

## FaceTime

    //FaceTime设置界面
    NSURL *url = [NSURL URLWithString:@"prefs:root=FACETIME"];
    if ([[UIApplication sharedApplication] canOpenURL:url])
    {
        [[UIApplication sharedApplication] openURL:url];
    }


## 音乐

    //音乐设置界面
    NSURL *url = [NSURL URLWithString:@"prefs:root=MUSIC"];
    if ([[UIApplication sharedApplication] canOpenURL:url])
    {
        [[UIApplication sharedApplication] openURL:url];
    }


## 墙纸设置界面

    //墙纸设置界面
    NSURL *url = [NSURL URLWithString:@"prefs:root=Wallpaper"];
    if ([[UIApplication sharedApplication] canOpenURL:url])
    {
        [[UIApplication sharedApplication] openURL:url];
    }


## 蓝牙设置界面

    //蓝牙设置界面
    NSURL *url = [NSURL URLWithString:@"prefs:root=Bluetooth"];
    if ([[UIApplication sharedApplication] canOpenURL:url])
    {
       [[UIApplication sharedApplication] openURL:url];
    }


## iCloud设置界面

    //iCloud设置界面
    NSURL *url = [NSURL URLWithString:@"prefs:root=CASTLE"];
    if ([[UIApplication sharedApplication] canOpenURL:url]
    {
       [[UIApplication sharedApplication] openURL:url];
    }


## 参数配置

看到这几个例子，大家有没有发现，想跳到哪个设置界面只需要prefs:root=后面的值即可！是的，就是这样的。

我在网上找到一个列表，可以跳到这些界面的参数配置：

    About — prefs:root=General&path=About
    Accessibility — prefs:root=General&path=ACCESSIBILITY
    Airplane Mode On — prefs:root=AIRPLANE_MODE
    Auto-Lock — prefs:root=General&path=AUTOLOCK
    Brightness — prefs:root=Brightness
    Bluetooth — prefs:root=General&path=Bluetooth
    Date & Time — prefs:root=General&path=DATE_AND_TIME
    FaceTime — prefs:root=FACETIME
    General — prefs:root=General
    Keyboard — prefs:root=General&path=Keyboard
    iCloud — prefs:root=CASTLE
    iCloud Storage & Backup — prefs:root=CASTLE&path=STORAGE_AND_BACKUP
    International — prefs:root=General&path=INTERNATIONAL
    Location Services — prefs:root=LOCATION_SERVICES
    Music — prefs:root=MUSIC
    Music Equalizer — prefs:root=MUSIC&path=EQ
    Music Volume Limit — prefs:root=MUSIC&path=VolumeLimit
    Network — prefs:root=General&path=Network
    Nike + iPod — prefs:root=NIKE_PLUS_IPOD
    Notes — prefs:root=NOTES
    Notification — prefs:root=NOTIFICATIONS_ID
    Phone — prefs:root=Phone
    Photos — prefs:root=Photos
    Profile — prefs:root=General&path=ManagedConfigurationList
    Reset — prefs:root=General&path=Reset
    Safari — prefs:root=Safari
    Siri — prefs:root=General&path=Assistant
    Sounds — prefs:root=Sounds
    Software Update — prefs:root=General&path=SOFTWARE_UPDATE_LINK
    Store — prefs:root=STORE
    Twitter — prefs:root=TWITTER
    Usage — prefs:root=General&path=USAGE
    VPN — prefs:root=General&path=Network/VPN
    Wallpaper — prefs:root=Wallpaper
    Wi-Fi — prefs:root=WIFI

