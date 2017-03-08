---
layout: post
title: "iOS 一句话调用清除缓存"
date: 2017-01-19
author: "Asingers"
header-img: http://7xqmgj.com1.z0.glb.clouddn.com/2016-11-29-Wallions22023.jpeg
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
      
---

<iframe src="//banners.itunes.apple.com/banner.html?partnerId=&aId=1001ludq&bt=catalog&t=catalog_white&id=1177953618&c=us&l=en-US&w=728&h=90&store=apps" frameborder=0 style="overflow-x:hidden;overflow-y:hidden;width:728px;height:90px;border:0px"></iframe>

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
	
	#import "YJCleanCache.h"

	@implementation YJCleanCache
	/**
	 *  清理缓存
	 */
	+(void)cleanCache:(cleanCacheBlock)block
	{
			dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
					//文件路径
					NSString *directoryPath=[NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];

					NSArray *subpaths = [[NSFileManager defaultManager] contentsOfDirectoryAtPath:directoryPath error:nil];

					for (NSString *subPath in subpaths) {
							NSString *filePath = [directoryPath stringByAppendingPathComponent:subPath];
							[[NSFileManager defaultManager] removeItemAtPath:filePath error:nil];
					}
					//返回主线程
					dispatch_async(dispatch_get_main_queue(), ^{
							block();
					});
			});

	}
	/**
	 *  计算整个目录大小
	 */
	+(float)folderSizeAtPath
	{
			NSString *folderPath=[NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];

			NSFileManager * manager=[NSFileManager defaultManager ];
			if (![manager fileExistsAtPath :folderPath]) {
					return 0 ;
			}
			NSEnumerator *childFilesEnumerator = [[manager subpathsAtPath :folderPath] objectEnumerator ];
			NSString * fileName;
			long long folderSize = 0 ;
			while ((fileName = [childFilesEnumerator nextObject ]) != nil ){
					NSString * fileAbsolutePath = [folderPath stringByAppendingPathComponent :fileName];
					folderSize += [ self fileSizeAtPath :fileAbsolutePath];
			}

			return folderSize/( 1024.0 * 1024.0 );
	}
	/**
	 *  计算单个文件大小
	 */
	+(long long)fileSizeAtPath:(NSString *)filePath{

			NSFileManager *manager = [NSFileManager defaultManager];

			if ([manager fileExistsAtPath :filePath]){

					return [[manager attributesOfItemAtPath :filePath error : nil ] fileSize];
			}
			return 0 ;

	}

	@end
	
	[YJCleanCache folderSizeAtPath];

