---
layout: post
title: "iOS使用UIActivityViewController"
date: 2016-08-09
author: "Alpaca"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
   - 开发
   
---
用法有很多,先实现AirDrop
直接上代码: 


    #import "DocumentViewController.h"

    @interface DocumentViewController ()

    @end

    @implementation DocumentViewController

    - (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
    {
        self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
        if (self) {
            // Custom initialization
        }
        return self;
    }

    - (void)viewDidLoad
    {
        [super viewDidLoad];
        // 展示用
        NSURL *url = [self fileToURL:self.documentName];
        NSURLRequest *urlRequest = [NSURLRequest requestWithURL:url];
        [self.webView loadRequest:urlRequest];
    
    }

    - (void)didReceiveMemoryWarning
    {
        [super didReceiveMemoryWarning];
        // Dispose of any resources that can be recreated.
    }

    - (NSURL *) fileToURL:(NSString*)filename
    {
        NSArray *fileComponents = [filename componentsSeparatedByString:@"."];
        NSString *filePath = [[NSBundle mainBundle] pathForResource:[fileComponents     objectAtIndex:0] ofType:[fileComponents objectAtIndex:1]];
    
        return [NSURL fileURLWithPath:filePath];
    }
    // 点击分享
    - (IBAction)share:(id)sender {
        NSURL *url = [self fileToURL:self.documentName];
        NSArray *objectsToShare = @[url];
    
        UIActivityViewController *controller = [[UIActivityViewController alloc]     initWithActivityItems:objectsToShare applicationActivities:nil];
    
        // Exclude all activities except AirDrop.
        NSArray *excludedActivities = @[UIActivityTypePostToTwitter, UIActivityTypePostToFacebook,
                                    UIActivityTypePostToWeibo,
                                    UIActivityTypeMessage, UIActivityTypeMail,
                                    UIActivityTypePrint, UIActivityTypeCopyToPasteboard,
                                    UIActivityTypeAssignToContact, UIActivityTypeSaveToCameraRoll,
                                    UIActivityTypeAddToReadingList, UIActivityTypePostToFlickr,
                                    UIActivityTypePostToVimeo, UIActivityTypePostToTencentWeibo];
        controller.excludedActivityTypes = excludedActivities;
    
        // Present the controller
        [self presentViewController:controller animated:YES completion:nil];
    
    }
    @end  
    
    
URL就不说了,目的就是通过文件名找到要分享的文件.作为参数.  
通过简单的代码就能够实现AirDrop分享文件.