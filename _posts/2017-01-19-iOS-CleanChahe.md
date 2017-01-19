---
layout: post
title: iOS 一句话调用清除缓存
description: 学习笔记
categories: iOS
tags: iOS

---
	#import <Foundation/Foundation.h>
	typedef void(^cleanCacheBlock)();

	@interface YJCleanCache : NSObject
	/**
	 *  清理缓存
	 */
	+(void)cleanCache:(cleanCacheBlock)block;

	/**
	 *  整个缓存目录的大小
	 */
	+(float)folderSizeAtPath;
	@end