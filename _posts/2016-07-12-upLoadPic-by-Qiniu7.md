---
layout: post
title: "七牛上传图片Demo iOS"
date: 2016-07-12
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/201607121.jpeg"
author: "Asingers"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
---

#### 首先是针对iOS 7 及更高,iOS 6版本日后再叙,稍有区别.  
  
#### 平时用到网络图片比较多,写博客就是.所以写了个Demo,这样做的目的是方便上传手机内的图片,这才是重点.

- [下载对应 SDK](http://developer.qiniu.com/code/v7/sdk/objc.html)  

图片选择器配合第三方,主要效果: 
主界面  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-13_Simulator%20Screen%20Shot%202016%E5%B9%B47%E6%9C%8813%E6%97%A5%20%E4%B8%8B%E5%8D%884.16.42.png" alt="" class="shadow"/>  

图片选择和拍照  

<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-13_Simulator%20Screen%20Shot%202016%E5%B9%B47%E6%9C%8813%E6%97%A5%20%E4%B8%8B%E5%8D%884.17.12.png" alt="" class="shadow"/>  

上传成功得到地址  
<img src="http://7xqmgj.com1.z0.glb.clouddn.com/2016-07-13_IMG_3168.PNG" alt="" class="shadow"/>  


##### 主要工作  
- 申请Key 创建空间 ...
- 生成TOKEN 客户端生成,我们并不需要服务器
- 初始化上传策略 (相关配置)
- 上传 (多张图片可以多次上传)

##### 完整Demo中包含个人信息,整理后放出.

- #### 注  
`系统方法图片上传`

#### 上传照片

	- (void)positiveAction:(UIButton *)btn{
    self.btn = btn;
    UIImagePickerController *imagePickerController =
    [[UIImagePickerController alloc] init];
    imagePickerController.delegate = self;
    imagePickerController.allowsEditing = YES;
    if (btn.tag == 10000 || btn.tag == 20000) {
        imagePickerController.sourceType = UIImagePickerControllerSourceTypePhotoLibrary;
    }else{
        imagePickerController.sourceType = UIImagePickerControllerSourceTypeCamera;
    }
    [self.navigationController presentViewController:imagePickerController
                                            animated:YES
                                          completion:^{}];
	}		
	}
	
##### pragma mark - image picker delegte
	- (void)imagePickerController:(UIImagePickerController *)picker
        didFinishPickingImage:(UIImage *)image
                  editingInfo:(NSDictionary<NSString *, id> *)editingInfo{
    [picker dismissViewControllerAnimated:YES completion:^{}];
    //上传
    MBProgressHUD *hud = [MBProgressHUD showHUDAddedTo:self.view animated:YES];
    hud.mode = MBProgressHUDModeIndeterminate;
    hud.labelText = @"正在加载";
    
    AFHTTPRequestOperationManager * manager = [AFHTTPRequestOperationManager manager];
    manager.responseSerializer = [AFHTTPResponseSerializer serializer];
    NSData * data = UIImageJPEGRepresentation(image, 0.1);
    NSUserDefaults * user = [NSUserDefaults standardUserDefaults];
    NSDictionary * dic = @{@"id":[user objectForKey:@"id"],
                           @"app_token":[user objectForKey:@"app_token"],
                           @"image":data};
    
    if (self.btn.tag == 10000 || self.btn.tag == 10001) {
        
        [manager POST:@"http://labour.chinadeer.cn/api/user/upload/image"
           parameters:dic constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
               
               [formData appendPartWithFileData:data
                                           name:@"image"
                                       fileName:@"front_image.jpg"
                                       mimeType:@"image/jpeg"];
               
           } success:^(AFHTTPRequestOperation *operation, id responseObject) {
               [hud setHidden:YES];
               id jDic = [NSJSONSerialization JSONObjectWithData:responseObject
                                                         options:NSJSONReadingMutableLeaves
                                                           error:nil];
               if ([jDic[@"code"] integerValue]== 200) {
                   frontImage = image;
                   if (backImage) {
                       self.idV.upBtn.enabled = YES;
                       self.idV.upBtn.alpha = 1;
                   }
                   _idV.positiveView.positivePhoto.image = image;
                   [self.info setObject:jDic[@"data"][@"id"] forKey:@"card_image_front"];
               }else{
                   NSLog(@"失败信息:%@",jDic[@"data"]);
               }
           } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
               [hud setHidden:YES];
               NSLog(@"失败:%@",error);
               
           }];
    }else if (self.btn.tag == 20000 || self.btn.tag == 20001){
        
        [manager POST:@"http://labour.chinadeer.cn/api/user/upload/image"
           parameters:dic constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
               
               [formData appendPartWithFileData:data name:@"image"
                                       fileName:@"back_image.jpg"
                                       mimeType:@"image/jpeg"];
               
           } success:^(AFHTTPRequestOperation *operation, id responseObject) {
               [hud setHidden:YES];
               id jDic = [NSJSONSerialization JSONObjectWithData:responseObject
                                                         options:NSJSONReadingMutableLeaves
                                                           error:nil];
               if ([jDic[@"code"] integerValue] == 200) {
                   backImage = image;
                   if (frontImage) {
                       self.idV.upBtn.enabled = YES;
                       self.idV.upBtn.alpha = 1;
                   }
                    _idV.onTheBackView.positivePhoto.image = image;
                   [self.info setObject:jDic[@"data"][@"id"] forKey:@"card_image_back"];
               }else{
                   NSLog(@"失败信息:%@",jDic[@"data"]);
               }
           } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
               [hud setHidden:YES];
               NSLog(@"失败:%@",error);
           }];
    }
	}

	- (void)imagePickerControllerDidCancel:(UIImagePickerController *)picker{
	
    [self dismissViewControllerAnimated:YES completion:^{}];
    
    }


 







