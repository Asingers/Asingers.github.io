---
layout: post
title: iOS iCloud Development(3) iCloudKit Storage
subtitle: iCloud 存储之 iCloudKit
categories: ios
header-mask: 0.7
tags: 
    - iOS

---
   

cloud kit一共有7个基础类，简单了解一下这些基础类接下来的使用就比较容易理解了。

### CKContainer ###

`CKContainer`类似于应用运行时的沙盒，一个应用只能访问自己的沙盒，同样的，一个应用也只能访问自己的Container。

通过初始化之后就可以使用

```

 CKContainer *container = [CKContainer defaultContainer];

```

### CKDatabase ###

`CKDatabase`很明显就是数据库，他拥有***私有数据库***和***公有数据库***两种类型。用户只能访问自己的私有数据库，一些不敏感的数据也可以存储在公有数据库中。

```

 //公有数据库

 CKDatabase *datebase = container.publicCloudDatabase;

 //私有数据库

 CKDatabase *datebase = container.privateCloudDatabase;

```

### CKRecord ###

`CKRecord`就是数据库中的一条数据记录，他通过key-value的方式来存储和获取数据。目前可以支持如下格式的数据:

- 
NSString（swift中的Sting）

- 
NSNumber

- 
NSData

- 
NSDate

- 
CLLocation

- 
CKReference

- 
CKAsset

其中图片的数据类型需要先初始化一个URL，然后把图片保存到本地沙盒，生成URL，再创建`CKAsset`来存储。

### CKRecordZone ###

`CKRecordZone`是用来保存`Record`的。所有的`Record`都是保存在这里，应用有一个默认的`zone`，也可以自定义`zone`。

### CKRecordIdentifier ###

`CKRecordIdentifier`是`Record`的唯一标示，用来确定`Record`在数据库中的位置。

### CKReference ###

`CKReference`是一种引用关系。

### CKAsset ###

`CKAsset`  为资源文件，比如之前提到的照片就是用这种方式存储的。

## CloudKit Dashboard ##
在Xcode中进入或者直接在开发者网站

### Data ###

Cloud kit的数据管理中心入口。在这里可以进行数据的字段，索引等内容设定。

### Logs ###

查看Cloud kit的服务日志，显示数据库操作，推送通知以及对应环境中的其他活动。

### Telemetry ###

查看对应环境中服务器端性能和数据库利用率的图表。

### Public Database Usage ###

查看公共数据库的使用情况图标，包括活跃用户、请求频率等。

### Api Access ###

管理API令牌和服务器密钥，允许对应环境进行web服务调用。

### 使用 ###

这次我们只讲简单的操作，所以本篇文章就只使用 Data中的部分功能。

#### 创建 RECORD TYPE ####

首先点击**Data** , **RECORD TYPE** , **Create New Type**创建一个新的RECORD TYPE。

![](/images/post/20190124/icloud1.png)

这里*注意*，我们的RECORD TYPE Name 是*Note*，这个在之后代码里操作非常重要，所以记得不要写错了。

#### 添加 Field ####

有了表，接下来就是添加字段了。点击*Add Field*  然后输入字段名，选择类型就可以了。这里可以一次性添加多条，添加完点击保存就可以了。
![](/images/post/20190124/icloud2.png)

添加的内容有：

- 
title  String

- 
content  String

- 
photo  Asset

#### 添加 indexs ####

为了搜索更方便一些，我们添加一些索引。

点击**INDEXS**，选择我们刚刚添加的**Note**，然后添加索引，添加的索引类型有**QUERYABLE**,**SEARCHABLE**,**SORTABLE**。

![](/images/post/20190124/icloud3.png)


添加的内容有：

- 
title  QUERYABLE

- 
title  SEARCHABLE

- 
title  SORTABLE

- 
content  QUERYABLE

- 
content  SEARCHABLE

- 
content  SORTABLE

- 
recordName QUERYABLE


#### 添加数据 ####

接下来，我们就可以给表里添加数据了。

选择**RECORD** ,确认**LOAD RECORDS FROM**和**QUERY FOR RECORDS OF TYPE**  都正确了之后，点击**Creat New Record...**，在右边输入想要插入的数据，添加个几条就可以了。

![](/images/post/20190124/icloud4.png)


#### 查询数据 ####

随便添加几条数据之后，还是在这个页面，点击左侧的**Query Records**，就可以查询到数据了。

## 通过代码操作 Cloud Kit ##

使用Cloud Kit时，首先要先引入Cloud kit的框架。
`#import <CloudKit/CloudKit.h>
`

```
//获取最新的数据
+ (void)getNewDocument:(NSMetadataQuery *)myMetadataQuery
{
    [myMetadataQuery setSearchScopes:@[NSMetadataQueryUbiquitousDocumentsScope]];
    [myMetadataQuery startQuery];
}


#pragma mark - Cloud Kit

+ (void)saveCloudKitModelWithTitle:(NSString *)title content:(NSString *)content photoImage:(UIImage *)image
{
    CKContainer *container = [CKContainer defaultContainer];

    //公共数据
    CKDatabase *datebase = container.publicCloudDatabase;
//    //私有数据
//    CKDatabase *datebase = container.privateCloudDatabase;

    //创建主键
//    CKRecordID *noteID = [[CKRecordID alloc] initWithRecordName:@"NoteID"];
    
    //创建保存数据
    CKRecord *noteRecord = [[CKRecord alloc] initWithRecordType:RECORD_TYPE_NAME];
    
    
    NSData *imageData = UIImagePNGRepresentation(image);
    if (imageData == nil)
    {
        imageData = UIImageJPEGRepresentation(image, 0.6);
    }
    NSString *tempPath = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents/imagesTemp"];
    NSFileManager *manager = [NSFileManager defaultManager];
    if (![manager fileExistsAtPath:tempPath]) {
        
        [manager createDirectoryAtPath:tempPath withIntermediateDirectories:YES attributes:nil error:nil];
    }
    
    NSDate *dateID = [NSDate dateWithTimeIntervalSinceNow:0];
    NSTimeInterval timeInterval = [dateID timeIntervalSince1970] * 1000;      //*1000表示到毫秒级，这样可以保证不会同时生成两个同样的id
    NSString *idString = [NSString stringWithFormat:@"%.0f", timeInterval];

    NSString *filePath = [NSString stringWithFormat:@"%@/%@",tempPath,idString];
    NSURL *url = [NSURL fileURLWithPath:filePath];
    [imageData writeToURL:url atomically:YES];
    
    CKAsset *asset = [[CKAsset alloc]initWithFileURL:url];
    
    [noteRecord setValue:title forKey:@"title"];
    [noteRecord setValue:content forKey:@"content"];
    [noteRecord setValue:asset forKey:@"image"];
    
    
    [datebase saveRecord:noteRecord completionHandler:^(CKRecord * _Nullable record, NSError * _Nullable error) {
        
        if(!error)
        {
            NSLog(@"保存成功");
        }
        else
        {
            NSLog(@"保存失败");
            NSLog(@"%@",error.description);
        }
    }];
}


```

#### 查询数据
##### 查询所有数据
查询数据同样，我们也要先获取容器和数据库。

```
/查询数据
+ (void)queryCloudKitData
{
    //获取位置
    CKContainer *container = [CKContainer defaultContainer];
    CKDatabase *database = container.publicCloudDatabase;
    
    //添加查询条件
    NSPredicate *predicate = [NSPredicate predicateWithValue:YES];
    CKQuery *query = [[CKQuery alloc] initWithRecordType:RECORD_TYPE_NAME predicate:predicate];
    query.sortDescriptors = @[[NSSortDescriptor sortDescriptorWithKey:@"collectTime" ascending:NO]];
    //开始查询
    [database performQuery:query inZoneWithID:nil completionHandler:^(NSArray<CKRecord *> * _Nullable results, NSError * _Nullable error) {
        
        NSLog(@"%@",results);
        //把数据做成字典通知出去
        NSDictionary *userinfoDic = [NSDictionary dictionaryWithObject:results forKey:@"key"];

        [[NSNotificationCenter defaultCenter] postNotificationName:@"CloudDataQueryFinished" object:nil userInfo:userinfoDic];
    }];

}
```

#### 查询单个数据

```
//查询单条数据
+ (void)querySingleRecordWithRecordID:(CKRecordID *)recordID
{
    //获取容器
    CKContainer *container = [CKContainer defaultContainer];
    //获取公有数据库
    CKDatabase *database = container.publicCloudDatabase;
    
    [database fetchRecordWithID:recordID completionHandler:^(CKRecord * _Nullable record, NSError * _Nullable error) {
        
        dispatch_async(dispatch_get_main_queue(), ^{
            NSLog(@"%@",record);
            //把数据做成字典通知出去
            NSDictionary *userinfoDic = [NSDictionary dictionaryWithObject:record forKey:@"key"];
            [[NSNotificationCenter defaultCenter] postNotificationName:@"CloudDataSingleQueryFinished" object:nil userInfo:userinfoDic];
        });
    }];
}
```

### 删除数据

```
+ (void)removeCloudKitDataWithRecordID:(CKRecordID *)recordID
{
    CKContainer *container = [CKContainer defaultContainer];
    CKDatabase *database = container.publicCloudDatabase;
    
    [database deleteRecordWithID:recordID completionHandler:^(CKRecordID * _Nullable recordID, NSError * _Nullable error) {
        NSLog(@"删除成功");
    }];

}
```

#### 修改数据

```
//修改数据
+ (void)changeCloudKitWithTitle:(NSString *)title content:(NSString *)content photoImage:(UIImage *)image RecordID:(CKRecordID *)recordID
{
    //获取容器
    CKContainer *container = [CKContainer defaultContainer];
    //获取公有数据库
    CKDatabase *database = container.publicCloudDatabase;
    
    [database fetchRecordWithID:recordID completionHandler:^(CKRecord * _Nullable record, NSError * _Nullable error) {
        
        
        NSData *imageData = UIImagePNGRepresentation(image);
        if (imageData == nil)
        {
            imageData = UIImageJPEGRepresentation(image, 0.6);
        }
        NSString *tempPath = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents/imagesTemp"];
        NSFileManager *manager = [NSFileManager defaultManager];
        if (![manager fileExistsAtPath:tempPath]) {
            
            [manager createDirectoryAtPath:tempPath withIntermediateDirectories:YES attributes:nil error:nil];
        }
        
        NSDate *dateID = [NSDate dateWithTimeIntervalSinceNow:0];
        NSTimeInterval timeInterval = [dateID timeIntervalSince1970] * 1000;      //*1000表示到毫秒级，这样可以保证不会同时生成两个同样的id
        NSString *idString = [NSString stringWithFormat:@"%.0f", timeInterval];
        
        NSString *filePath = [NSString stringWithFormat:@"%@/%@",tempPath,idString];
        NSURL *url = [NSURL fileURLWithPath:filePath];
        [imageData writeToURL:url atomically:YES];
        CKAsset *asset = [[CKAsset alloc]initWithFileURL:url];
        [record setObject:title forKey:@"title"];
        [record setObject:content forKey:@"content"];
        [record setValue:asset forKey:@"photo"];
        [database saveRecord:record completionHandler:^(CKRecord * _Nullable record, NSError * _Nullable error) {
            
            if(error)
            {
                
                NSLog(@"修改失败 %@",error.description);
            }
            else
            {
                NSLog(@"修改成功");
            }
        }];
    }];
}
```



