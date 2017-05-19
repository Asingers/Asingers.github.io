---
layout: post
title: "MJExtension 整理"
date: 2016-05-31
author: "Alpaca"
subtitle: "学习笔记"
catalog: true
categories: ios
tags:
   - iOS
---


### <a id="JSON_Model"></a> The most simple JSON -> Model【最简单的字典转模型】  
```  
objc
typedef enum {
    SexMale,
    SexFemale
} Sex;

@interface User : NSObject
@property (copy, nonatomic) NSString *name;
@property (copy, nonatomic) NSString *icon;
@property (assign, nonatomic) unsigned int age;
@property (copy, nonatomic) NSString *height;
@property (strong, nonatomic) NSNumber *money;
@property (assign, nonatomic) Sex sex;
@property (assign, nonatomic, getter=isGay) BOOL gay;
@end

/***********************************************/

NSDictionary *dict = @{
    @"name" : @"Jack",
    @"icon" : @"lufy.png",
    @"age" : @20,
    @"height" : @"1.55",
    @"money" : @100.9,
    @"sex" : @(SexFemale),
    @"gay" : @"true"
//   @"gay" : @"1"
//   @"gay" : @"NO"
};

// JSON -> User
User *user = [User mj_objectWithKeyValues:dict];

NSLog(@"name=%@, icon=%@, age=%zd, height=%@, money=%@, sex=%d, gay=%d", user.name, user.icon, user.age, user.height, user.money, user.sex, user.gay);
// name=Jack, icon=lufy.png, age=20, height=1.550000, money=100.9, sex=1  
 
```


### <a id="JSON_array_model_array"></a> JSON array -> model array【将一个字典数组转成模型数组】

```  
objc
NSArray *dictArray = @[
                         @{
                             @"name" : @"Jack",
                             @"icon" : @"lufy.png"
                         },
                         @{
                             @"name" : @"Rose",
                             @"icon" : @"nami.png"
                         }
                     ];

// JSON array -> User array
NSArray *userArray = [User mj_objectArrayWithKeyValuesArray:dictArray];

// Printing
for (User *user in userArray) {
    NSLog(@"name=%@, icon=%@", user.name, user.icon);
}
// name=Jack, icon=lufy.png
// name=Rose, icon=nami.png  

```
