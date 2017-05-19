---
layout: post
title: SDWebImage 缓存机制
description: SDWebImage 缓存机制
categories: ios
tags: iOS
catalog: true

---
> SDWebImage 相信对大多数开发者来说，都是一个不陌生的名字。它除了帮助我们读取网络图片，还会处理这些图片的缓存。它的缓存机制到底是什么样的呢，今天我们就来看一看.

### 基本结构
![](http://cloud9dic.b0.upaiyun.com/2017-04-12-021259.jpg)  

SDWebImage 有很多类,有一个专门的 Cache 分类用来处理图片的缓存。 这里面也有两个类 SDImageCache 和 SDImageCacheConfig。 大部分的缓存处理都在 SDImageCache 这个类中实现。  

### Memory 和 Disk 双缓存
首先，SDWebImage 的图片缓存采用的是 Memory 和 Disk 双重 Cache 机制.先来看一段代码:

	@interface SDImageCache ()
	#pragma mark - Properties
	@property (strong, nonatomic, nonnull) NSCache *memCache;
	...
	
这里我们发现， 有一个叫做 memCache 的属性，它是一个 NSCache 对象，用于实现我们对图片的 Memory Cache。 SDWebImage 还专门实现了一个叫做 AutoPurgeCache 的类， 相比于普通的 NSCache， 它提供了一个在内存紧张时候释放缓存的能力：  

	@interface AutoPurgeCache : NSCache
	@end
	@implementation AutoPurgeCache
	- (nonnull instancetype)init {
    	self = [super init];
    	if (self) {
	#if SD_UIKIT
        	[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(removeAllObjects) name:UIApplicationDidReceiveMemoryWarningNotification object:nil];
	#endif
    	}
    	return self;
	}  

其实就是接受系统的内存警告通知，然后清除掉自身的图片缓存。 这里大家比较少见的一个类应该是 NSCache 了。 简单来说，它是一个类似于 NSDictionary 的集合类，用于在内存中存储我们要缓存的数据。详细信息大家可以参考官方文档：https://developer.apple.com/reference/foundation/nscache。

还有一个就是Disk Cache，也就是文件缓存。  
SDWebImage 会将图片存放到 NSCachesDirectory 目录中：

	- (nullable NSString *)makeDiskCachePath:(nonnull NSString*)fullNamespace {
    NSArray<NSString *> *paths = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES);
    return [paths[0] stringByAppendingPathComponent:fullNamespace];
	}

然后为每一个缓存文件生成一个 md5 文件名, 存放到文件中。  

### 整体机制
通常用法流程:  
加载:  

	[imageView sd_setImageWithURL:[NSURL URLWithString:@"http://cloud9dic.b0.upaiyun.com/2017-04-12-021259.jpg"]];

首先这个 Category 方法 sd_setImageWithURL 内部会调用 SDWebImageManager 的 downloadImageWithURL 方法来处理这个图片 URL：  

	id <SDWebImageOperation> operation = [SDWebImageManager.sharedManager downloadImageWithURL:url options:options progress:progressBlock completed:^(UIImage *image, NSError *error, SDImageCacheType cacheType, BOOL finished, NSURL *imageURL) {
            ...
	}];  
	
SDWebImageManager 内部的 downloadImageWithURL 方法会先使用我们前面提到的 SDImageCache 类的 queryDiskCacheForKey 方法，查询图片缓存：  

	operation.cacheOperation = [self.imageCache queryDiskCacheForKey:key done:^(UIImage *image, SDImageCacheType cacheType) {
	...
	}];  
	
再来看 queryDiskCacheForKey 方法内部， 先会查询 Memory Cache ：  

	UIImage *image = [self imageFromMemoryCacheForKey:key];
	if (image) {
	doneBlock(image, SDImageCacheTypeMemory);
    	return nil;
	}   
	
如果 Memory Cache 查找不到， 就会查询 Disk Cache:  

	dispatch_async(self.ioQueue, ^{
    if (operation.isCancelled) {
        return;
    }
    @autoreleasepool {
        UIImage *diskImage = [self diskImageForKey:key];
        if (diskImage && self.shouldCacheImagesInMemory) {
            NSUInteger cost = SDCacheCostForImage(diskImage);
            [self.memCache setObject:diskImage forKey:key cost:cost];
        }
        dispatch_async(dispatch_get_main_queue(), ^{
            doneBlock(diskImage, SDImageCacheTypeDisk);
        });
    }
	});  
	
如果 Disk Cache 查询成功，还会把得到的图片再次设置到 Memory Cache 中。 这样做可以最大化那些高频率展现图片的效率。

如果缓存查询成功， 那么就会直接返回缓存数据。 如果不成功，接下来就开始请求网络：  

	id <SDWebImageOperation> subOperation = [self.imageDownloader downloadImageWithURL:url options:downloaderOptions progress:progressBlock completed:^(UIImage *downloadedImage, NSData *data, NSError *error, BOOL finished) {
	}  
	
请求网络使用的是 imageDownloader 属性，这个示例专门负责下载图片数据。 如果下载失败， 会把失败的图片地址写入 failedURLs 集合：  

	if (   error.code != NSURLErrorNotConnectedToInternet
    && error.code != NSURLErrorCancelled
    && error.code != NSURLErrorTimedOut
    && error.code != NSURLErrorInternationalRoamingOff
    && error.code != NSURLErrorDataNotAllowed
    && error.code != NSURLErrorCannotFindHost
    && error.code != NSURLErrorCannotConnectToHost) {
    @synchronized (self.failedURLs) {
        [self.failedURLs addObject:url];
    	}
	}  
	
为什么要有这个 failedURLs 呢， 因为 SDWebImage 默认会有一个对上次加载失败的图片拒绝再次加载的机制。 也就是说，一张图片在本次会话加载失败了，如果再次加载就会直接拒绝。  

如果下载图片成功了，接下来就会使用 [self.imageCache storeImage] 方法将它写入缓存，并且调用 completedBlock 告诉前端显示图片：  

	if (downloadedImage && finished) {
    [self.imageCache storeImage:downloadedImage recalculateFromImage:NO imageData:data forKey:key toDisk:cacheOnDisk];
	}
	dispatch_main_sync_safe(^{
    if (strongOperation && !strongOperation.isCancelled) {
        completedBlock(downloadedImage, nil, SDImageCacheTypeNone, finished, url);
    	}
	});
	
### 是否要重试失败的 URL
你可以在加载图片的时候设置 SDWebImageRetryFailed 标记，这样 SDWebImage 就会加载之前失败过的图片了。 记得我们前面提到的 failedURLs 属性了吧，这个属性是在内存中存储的，如果图片加载失败， SDWebImage 会在本次 APP 会话中都不再重试这张图片了。当然这个加载失败是有条件的，如果是超时失败，不记在内。

总之，如果你更需要图片的可用性，而不是这一点点的性能优化，那么你就可以带上 SDWebImageRetryFailed 标记：  

	[_image sd_setImageWithURL:[NSURL URLWithString:@"http://cloud9dic.b0.upaiyun.com/2017-04-12-021259.jpg"] placeholderImage:nil options:SDWebImageRetryFailed];  
	
### Disk 缓存清理策略

SDWebImage 会在每次 APP 结束的时候执行清理任务。 清理缓存的规则分两步进行。 第一步先清除掉过期的缓存文件。 如果清除掉过期的缓存之后，空间还不够。 那么就继续按文件时间从早到晚排序，先清除最早的缓存文件，直到剩余空间达到要求。

具体点，SDWebImage 是怎么控制哪些缓存过期，以及剩余空间多少才够呢？ 通过两个属性：  

	@interface SDImageCache : NSObject
	@property (assign, nonatomic) NSInteger maxCacheAge;
	@property (assign, nonatomic) NSUInteger maxCacheSize;  
	
maxCacheAge 是文件缓存的时长， SDWebImage 会注册两个通知：  
	
	[[NSNotificationCenter defaultCenter] addObserver:self
                                                 selector:@selector(cleanDisk)
                                                     name:UIApplicationWillTerminateNotification
                                                   object:nil];
	[[NSNotificationCenter defaultCenter] addObserver:self
                                         selector:@selector(backgroundCleanDisk)
                                             name:UIApplicationDidEnterBackgroundNotification
                                           object:nil];  
                                           
分别在应用进入后台和结束的时候，遍历所有的缓存文件，如果缓存文件超过 maxCacheAge 中指定的时长，就会被删除掉。

同样的， maxCacheSize 控制 SDImageCache 所允许的最大缓存空间。 如果清理完过期文件后缓存空间依然没达到 maxCacheSize 的要求， 那么就会继续清理旧文件，直到缓存空间达到要求为止。  
了解了这个机制对我们有什么帮助呢? 我们来继续讲解，我们平时在使用 SDWebImage 的时候是没接触过它们的。那么以此推理，它们一定有默认值，也确实有：  

	static const NSInteger kDefaultCacheMaxCacheAge = 60 * 60 * 24 * 7; // 1 week

上面是 maxCacheAge 的默认值，注释上写的很清楚，缓存一周。 再来看看 maxCacheSize。 翻了一遍 SDWebImage 的代码，并没有对 maxCacheSize 设置默认值。 这就意味着 SDWebImage 在默认情况下不会对缓存空间设限制。

这一点可以在 SDWebImage 清理缓存的代码中求证：

	if (self.maxCacheSize > 0 && currentCacheSize > self.maxCacheSize) {
	//清理缓存代码
	}

说明一下， 上面代码中的 currentCacheSize 变量代表当前图片缓存占用的空间。 从这里可以看出， 只有在 maxCacheSize 大于 0 并且当前缓存空间大于 maxCacheSize 的时候才进行第二步的缓存清理。

这意味着什么呢？ 其实就是 SDWebImage 在默认情况下是不对我们的缓存大小设限制的，理论上，APP 中的图片缓存可以占满整个设备。

SDWebImage 的这个特性还是比较容易被大家忽略的，如果你开发的类似信息流的 APP，应该会加载大量的图片，如果这时候按照默认机制，缓存尺寸是没有限制的，并且默认的缓存周期是一周。 就很容易造成应用存储空间占用偏大的问题。  

另外，过多的占用缓存空间其实并不一定有用。大部分情况是一些图片被缓存下来后，很少再被重复展现。所以合理的规划缓存空间尺寸还是很有必要的。可以这样设置：
	
	[SDImageCache sharedImageCache].maxCacheSize = 1024 * 1024 * 50;	// 50M

maxCacheSize 是以字节来表示的，我们上面的计算代表 50M 的最大缓存空间。 把这行代码写在你的 APP 启动的时候，这样 SDWebImage 在清理缓存的时候，就会清理多余的缓存文件了。




