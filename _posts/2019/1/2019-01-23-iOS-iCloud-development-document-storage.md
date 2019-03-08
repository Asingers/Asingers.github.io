---
layout: post
title: iOS iCloud Development(2) Document Storage
subtitle: iCloud 存储之 Document
categories: ios
header-style: text
tags: 
    - iOS

---
   
#### NSFileManager
NSFileManager主要是对文件的操作，我们用它来获取iCloud的存储地址。
根据我们的entitlements，通过NSFileManager就可以获得iCloud的存储地址，在获取地址之后，我们要先判断一下获取的地址是否为空，如果这个地址为空，则说明用户的iCloud暂时不可用。

	/获取地址

	+ (NSURL *)getUbiquityContauneURLWithFileName:(NSString *)fileName

	{

	 NSURL *ubiquityURL = [[NSFileManager defaultManager] URLForUbiquityContainerIdentifier:UbiquityContainerIdentifiers];

	 //验证iCloud是否可用

	 if(!ubiquityURL)

	 {

	 NSLog(@"尚未开启iCloud功能");

	 return nil;

	 }

	 NSURL *URLWithFileName = [ubiquityURL URLByAppendingPathComponent:@"Documents"];

	 URLWithFileName = [URLWithFileName URLByAppendingPathComponent:fileName];

	 return URLWithFileName;

	}

#### UIDocument
UIDocument主要是用于对文件内容的操作。
获取了文件的地址之后，我们就可以直接对文件进行操作了，但是官方文档建议通过UIDocument来操作，因为当我们在对iCloud进行操作的时候，不止是只有我们自己对他进行操作，iCloud daemon也会对iCloud操作，用UIDocument操作能够保证存取安全。
在使用UIDocument之前，我们新建一个类，继承于UIDocument，并且重写两个方法：
	
	- (BOOL)loadFromContents:(id)contents ofType:(NSString *)typeName error:(NSError * _Nullable __autoreleasing *)outError

	{
	 self.myData = [contents copy];
	 return YES;
	}

	- (nullable id)contentsForType:(NSString *)typeName error:(NSError * _Nullable __autoreleasing *)outError

	{
	 if(!self.myData)
	 {
	 self.myData = [[NSData alloc] init];
	 }
	 return self.myData;
	}


#### NSMetadataQuery

NSMetadataQuery主要用来查询数据。

##### 创建文档
有了之前的准备工作，创建一个文档就非常简单了，只要创建好我们要保存的文件，通过调用

	- (void)saveToURL:(NSURL *)url forSaveOperation:(UIDocumentSaveOperation)saveOperation completionHandler:(void (^ __nullable)(BOOL success))completionHandler __TVOS_PROHIBITED;


##### 创建文档

	+ (void)createDocument

	{

	 NSString *fileName = @"test.txt";

	 NSURL *url = [iCloudHandle getUbiquityContauneURLWithFileName:fileName];

	 ZZRDocument *doc = [[ZZRDocument alloc] initWithFileURL:url];

	 NSString *docContent = @"iCloud Document 测试数据";

	 doc.myData = [docContent dataUsingEncoding:NSUTF8StringEncoding];

	 [doc saveToURL:url forSaveOperation:UIDocumentSaveForCreating completionHandler:^(BOOL success) {

	 if(success)
	 {
	 	NSLog(@"创建文档成功");
	 }
	 else{
	 	NSLog(@"创建文档失败");
	 }
	 	}];
	}

##### 修改文档
修改文档，其实就是重写文档，就是将上边创建文档中的UIDocumentSaveForCreating改为UIDocumentSaveForOverwriting。

	//修改文档 实际上是overwrite重写
	+ (void)overwriteDocument
	{
	 NSString *fileName = @"test.txt";
	 NSURL *url = [iCloudHandle getUbiquityContauneURLWithFileName:fileName];
	 ZZRDocument *doc = [[ZZRDocument alloc] initWithFileURL:url];
	 NSString *docContent = @"iCloud Document 修改数据";
	 doc.myData = [docContent dataUsingEncoding:NSUTF8StringEncoding];
	 [doc saveToURL:url forSaveOperation:UIDocumentSaveForOverwriting completionHandler:^(BOOL success) {
	 if(success)
	 {
	 	NSLog(@"修改文档成功");
	 }
	 else{
		 NSLog(@"修改文档失败");
	 }
	 	}];
	}

##### 删除文档
删除文档其实就是通过之前的地址获取到文件，然后调用remove方法即可。

	//删除文档

	+ (void)removeDocument
	{
	 	NSString *fileName = @"test.txt";
	 	NSURL *url = [iCloudHandle getUbiquityContauneURLWithFileName:fileName];
	 	NSError *error;
	 	[[NSFileManager defaultManager] removeItemAtURL:url error:&error];
		 if(error)
		 {
			 NSLog(@"删除文档失败 %@",error);
		 } else{
			 NSLog(@"删除文档成功");
		 }
	}
	
##### 查询文档
NSMetadataQuery。
	
	//获取最新的数据
	+ (void)getNewDocument:(NSMetadataQuery *)myMetadataQuery
	{
	 [myMetadataQuery setSearchScopes:@[NSMetadataQueryUbiquitousDocumentsScope]];
	 [myMetadataQuery startQuery];
	}
这个过程由系统接管，只需要注册相应通知就可以来接受数据通知

	/获取最新数据完成
	 [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(finishedGetNewDocument:) name:NSMetadataQueryDidFinishGatheringNotification object:self.myMetadataQuery];
	 
	 //数据更新通知
	 [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(documentDidChange:) name:NSMetadataQueryDidUpdateNotification object:self.myMetadataQuery];

##### 接收数据：


	- (void)finishedGetNewDocument:(NSMetadataQuery *)metadataQuery

	{

		 NSArray *item =self.myMetadataQuery.results;

		 [item enumerateObjectsUsingBlock:^(id _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {

		 NSMetadataItem *item = obj;

		 //获取文件名

		 NSString *fileName = [item valueForAttribute:NSMetadataItemFSNameKey];

	 //获取文件创建日期

		 NSDate *date = [item valueForAttribute:NSMetadataItemFSContentChangeDateKey];

		 NSLog(@"%@,%@",fileName,date);

		 ZZRDocument *doc = [[ZZRDocument alloc] initWithFileURL:[iCloudHandle getUbiquityContauneURLWithFileName:fileName]];

		 [doc openWithCompletionHandler:^(BOOL success) {
			 if(success)
	 {
		 NSLog(@"读取数据成功。");
		 NSString *docConten = [[NSString alloc] initWithData:doc.myData encoding:NSUTF8StringEncoding];
		 NSLog(@"%@",docConten);
	 }
		 }];
	 }];
	}

	- (void)documentDidChange:(NSMetadataQuery *)metadataQuery

	{
		 NSLog(@"Document 数据更新");
	}
