---
layout: post
title: "NSArray & NSMutableArray常用操作梳理"
subtitle: "深入理解"
date: 2016-02-22 
author: "Asingers"
header-img: "http://7xqmgj.com1.z0.glb.clouddn.com/header_imgnsarry.jpg"
categories: ios
tags:
    - iOS
    - Dev
---
Cocoa的NSArray是基于C底层CFArray/CFArrayRef实现的，NSArray可以看做是一个CFArrayRef的Wrapper类。**NSArrayI（Immutable）是NSArray的真正类型，**NSArrayM（Mutable）是NSMutableArray的真正类型。

NSArray保存的对象可以是不同类型的对象，但只能保存OC对象（继承自NSObject），int,char,double等基本C数据类型不能直接保存，需要通过装箱（boxing）成NSNumber、NSString或NSValue对象才能加入数组。

在苹果WWDC2012大会上介绍了大量Objective-C的新特性，其中有一点就是Object Literals，它允许你方便地基于字面量定义数字、数组和字典对象。

字面语法是的编译器指令，它提供简化符号来创建对象，类似于java5提供的auto boxing功能。这虽然是一个语法糖，但对提高写代码效率帮助很大。以下代码片段基于字面量语法快捷初始化数组（NSArray）：

    [objc] view plain copy
    print?
    
        NSString* yy = @"2015";  
        NSNumber* mm = @(07);  
        NSValue* dd = @(26);  
        NSArray* array = @[yy, mm, dd]; // 都是NSObject对象  
        NSLog(@"array = %@", array);  


## 1.创建初始化（Initialization&Creation）

Each object in array simply receives a retain message when it is added to the returned array using initWith*/arrayWith*method.

After an immutable array has been initialized in the following way, it cannot be modified.

1.1 Initializing an Array(NS_DESIGNATED_INITIALIZER)

    [objc] view plain copy
    print?
    
        // Initializes a newly allocated array. Not recommended for immutable array as  it's empty!  
        - (instancetype)init NS_DESIGNATED_INITIALIZER;  
        - (instancetype)initWithObjects:(const id[])objects count:(NSUInteger)cnt NS_DESIGNATED_INITIALIZER;  
        - (instancetype)initWithObjects:(id)firstObj, ... NS_REQUIRES_NIL_TERMINATION;  
        - (instancetype)initWithArray:(NSArray *)array;  
        // If YES, each object in array receives a copyWithZone: message to create a copy of the object instead of the retain message.  
        - (instancetype)initWithArray:(NSArray *)array copyItems:(BOOL)flag;  


以下是比较常用的初始化方法：

    - (instancetype)initWithObjects:(id)firstObj, ... NS_REQUIRES_NIL_TERMINATION;


1.2 Creating an Array(autorelease)

    [objc] view plain copy
    print?
    
        // Creates and returns an empty array. This method is used by mutable subclasses of NSArray.  
        + (instancetype)array;  
        + (instancetype)arrayWithObject:(id)anObject;  
        + (instancetype)arrayWithObjects:(const id[])objects count:(NSUInteger)cnt; // initWithObjects:count:  
        + (instancetype)arrayWithObjects:(id)firstObj, ...NS_REQUIRES_NIL_TERMINATION; // initWithObjects:count:  
        + (instancetype)arrayWithArray:(NSArray*)array; // initWithArray:  


以下是比较常用的类方法便利构造方法：

    + (instancetype)arrayWithObjects:(id)firstObj, ...NS_REQUIRES_NIL_TERMINATION; // initWithObjects:count:
    
    + (instancetype)arrayWithArray:(NSArray*)array; // initWithArray:


## 2.访问数组（Querying）

2.1 数组描述

    [objc] view plain copy
    print?
    
        @property (readonly,copy)NSString *description;  


例如以下代码可以在调试时打印数组：

    [objc] view plain copy
    print?
    
        NSArray* array = [NSArray arrayWithObjects:@"e0",@"e1",@"e2",@"e3",@"e4",@"e5",@"e6",nil];  
        NSLog(@"array = %@", array);  
        NSLog(@"array = %@", array.description);  


2.2 数组大小

    [objc] view plain copy
    print?
    
        //返回数组所包含的元素（NSObject对象）个数  
        @property (readonly)NSUInteger count;  


可以基于array.count对数组进行判空：如果array.count=0，则表示数组为nil或不包含任何元素。

2.3 数组元素

    [objc] view plain copy
    print?
    
        //返回数组第一个元素  
        @property (nonatomic,readonly)id firstObject NS_AVAILABLE(10_6,4_0);  
        @property (nonatomic,readonly)id lastObject;  
    
        //判断数组是否包含某个元素（按值查询）  
        - (BOOL)containsObject:(id)anObject;  
    
        //等效于objectAtIndex，支持中括号下标格式（array[index]）访问指定索引元素。  
        - (id)objectAtIndexedSubscript:(NSUInteger)idx NS_AVAILABLE(10_8,6_0);  
    
        //返回数组指定索引位置的元素，索引范围[0, count-1]  
        - (id)objectAtIndex:(NSUInteger)index;  
    
        //返回数组指定索引集的元素组成的子数组  
        - (NSArray *)objectsAtIndexes:(NSIndexSet *)indexes;  


- 
objectAtIndex:方法用于快速返回指定索引位置的元素；firstObject和lastObject属性用于快捷访问数组的首、尾元素。

- 
containsObject:方法用于按值搜索查询数组是否包含某个元素。



以下代码获取第2、4、6个元素子数组：

    [objc] view plain copy
    print?
    
        NSMutableIndexSet* indexSet = [NSMutableIndexSet indexSet];  
        [indexSet addIndex:1];  
        [indexSet addIndex:3];  
        [indexSet addIndex:5];  
        NSArray* subArray = [array objectsAtIndexes:indexSet];  
        NSLog(@"subArray= %@", subArray);  


等效于：

    [objc] view plain copy
    print?
    
        NSArray* subArray = [NSArray arrayWithObjects:[ array objectAtIndex:1], [array objectAtIndex:3], [array objectAtIndex:5], nil nil];  


2.4遍历数组

（1）索引遍历

    [objc] view plain copy
    print?
    
        // 倒序：for (NSInteger index=array.count-1; index>=0; index--)  
        for (NSUInteger index=0; index<array.count; index++)  
        {  
            NSLog(@"array[%zd] = %@", index, [array objectAtIndex:index]); // array[index]  
        }  


（2）枚举遍历

    [objc] view plain copy
    print?
    
        // 倒序：reverseObjectEnumerator  
        NSEnumerator* enumerator = [array objectEnumerator];  
        id e = nil;  
        while (e = [enumerator nextObject])  
        {  
            NSLog(@"e = %@", e);  
        }  
    
        /* 
        for (id e in enumerator) { 
            NSLog(@"e = %@",e); 
        } 
        */  


使用代码块传递遍历操作：

-(void)enumerateObjectsUsingBlock:(void (^)(id obj,NSUInteger idx,BOOL *stop))block NS_AVAILABLE(10_6,4_0);

    [objc] view plain copy
    print?
    
        // 示例1：枚举遍历  
        [array enumerateObjectsUsingBlock:^ (id obj, NSUInteger idx, BOOLBOOL *stop){  
            NSLog(@"obj = %@", obj);  
        }];  
    
        // 示例2：枚举遍历，遇到符合条件的元素即退出遍历。  
        [array enumerateObjectsUsingBlock:^ (id obj, NSUInteger idx, BOOLBOOL *stop){  
            if ([obj isEqualToString:@"e3"]) {  
                *stop = YES; // 中止遍历， break  
            } else {  
                *stop = NO; // 继续遍历，continue  
            }  
        }];  


以上版本默认是顺序同步遍历，另外一个版本可以指定NSEnumerationOptions参数：

    [objc] view plain copy
    print?
    
        typedefNS_OPTIONS(NSUInteger, NSEnumerationOptions) {  
            NSEnumerationConcurrent = (1UL <<0),// block并发  
            NSEnumerationReverse = (1UL <<1),//倒序  
        };  


（3）快速遍历

    [objc] view plain copy
    print?
    
        for (id e in array) {  
            NSLog(@"e = %@", e);  
        }  


## 3.查询数组（Finding）

3.1 indexOfObject(IdenticalTo)

    [objc] view plain copy
    print?
    
        // 在数组（或指定范围）中，测试指定的对象是否在数组中（按值查询）  
        - (NSUInteger)indexOfObject:(id)anObject; // 同containsObject  
        - (NSUInteger)indexOfObject:(id)anObject inRange:(NSRange)range;  
        // 测试指定的对象是否在数组中（按指针查询）  
        - (NSUInteger)indexOfObjectIdenticalTo:(id)anObject;  
        - (NSUInteger)indexOfObjectIdenticalTo:(id)anObject inRange:(NSRange)range;  


3.2 indexOfObject(s)PassingTest

使用代码块传递遍历操作过滤条件：

    [objc] view plain copy
    print?
    
        //查找数组中第一个符合条件的对象（代码块过滤），返回对应索引  
        - (NSUInteger)indexOfObjectPassingTest:(BOOL (^)(id obj,NSUInteger idx, BOOLBOOL *stop))predicate NS_AVAILABLE(10_6,4_0);  


以下代码用于获取值等于@”e3”的元素索引：

    [objc] view plain copy
    print?
    
        NSUInteger index = [array indexOfObjectPassingTest:^BOOL(id obj, NSUInteger idx, BOOLBOOL *stop) {  
            if ([obj isEqualToString:@"e3"]) {  
                return YES;  
                *stop = YES; // 中止遍历，break  
            } else {  
                *stop = NO; // 继续遍历，continue  
            }  
        }];  


查找数组中所有符合条件的对象（代码块过滤），返回对应索引集合：

    [objc] view plain copy
    print?
    
        - (NSIndexSet *)indexesOfObjectsPassingTest:(BOOL (^)(id obj,NSUInteger idx, BOOLBOOL *stop)) predicate NS_AVAILABLE(10_6,4_0);  


以上indexesOfObjectPassingTest/ indexesOfObjectsPassingTest版本默认是顺序同步遍历，它们都有另外可以指定NSEnumerationOptions参数的扩展版本。

indexOfObjectAtIndexes:options:passingTest:和indexOfObjectsAtIndexes:options:passingTest:则是指定索引集合内查找并返回索引（集合）。

3.3 firstObjectCommonWithArray

    [objc] view plain copy
    print?
    
        //查找与给定数组中第一个相同的对象（按值）  
        - (id)firstObjectCommonWithArray:(NSArray *)otherArray;  


示例：

    [objc] view plain copy
    print?
    
        id fo = [array firstObjectCommonWithArray:subArray];  
        NSLog(@"fo= %@", fo); // e1  


## 4.衍生数组（Deriving）

    [objc] view plain copy
    print?
    
        //返回指定范围（起始索引、长度）的子数组  
        -  (NSArray *)subarrayWithRange:(NSRange)range;  


以下代码获取数组前一半子数组：

    [objc] view plain copy
    print?
    
        //return the first half of the whole array  
        NSArray* subArray = [array subarrayWithRange:NSMakeRange(0,array.count/2)];  
        NSLog(@"subArray= %@", subArray);  
    
    [objc] view plain copy
    print?
    
        //在当前数组追加元素或数组，并返回新数组对象  
        - (NSArray *)arrayByAddingObject:(id)anObject;  
        - (NSArray *)arrayByAddingObjectsFromArray:(NSArray *)otherArray;  


5.可变数组（NSMutableArray）

5.1 Initializing an Array(NS_DESIGNATED_INITIALIZER)

    除了继承NSArray基本的init，还增加了以下指定初始化函数
    
    [objc] view plain copy
    print?
    
        - (instancetype)initWithCapacity:(NSUInteger)numItemsNS_DESIGNATED_INITIALIZER;  


5.2 addObject

    [objc] view plain copy
    print?
    
        //尾部追加一个元素  
        - (void)addObject:(id)anObject;  
        //尾部追加一个数组  
        - (void)addObjectsFromArray:(NSArray *)otherArray;  


5.3 insertObject

    [objc] view plain copy
    print?
    
        //在指定索引处插入一个元素，原来的元素后移  
        // index取值范围=[0, count]，index=count时相当于addObject  
        - (void)insertObject:(id)anObject atIndex:(NSUInteger)index;  
        //在指定索引集合处插入一个数组元素，相当于批次insertObject: atIndex:  
        - (void)insertObjects:(NSArray *)objects atIndexes:(NSIndexSet*)indexes;  


5.4 exchangeObject/replaceObject

    [objc] view plain copy
    print?
    
        //交换对应索引位置的元素（索引必须有效）  
        - (void)exchangeObjectAtIndex:(NSUInteger)idx1 withObjectAtIndex:(NSUInteger)idx2;  
    
        //替换对应索引位置的元素（索引必须有效）  
        - (void)replaceObjectAtIndex:(NSUInteger)index withObject:(id)anObject;  
        //替换对应索引集合位置的元素，相当于批次replaceObjectAtIndex: withObject:  
        - (void)replaceObjectsAtIndexes:(NSIndexSet *)indexes withObjects:(NSArray*)objects;  
    
        //等效于replaceObjectAtIndex，支持中括号下标格式（array[index]）赋值替换。  
        // index取值范围=[0, count]，index=count时相当于addObject  
        - (void)setObject:(id)obj atIndexedSubscript:(NSUInteger)idxNS_AVAILABLE(10_8,6_0);  
        //等效于先removeAllObjects后addObjectsFromArray  
        - (void)setArray:(NSArray *)otherArray;  


5.5 removeObject

    [objc] view plain copy
    print?
    
        - (void)removeLastObject;  
        //删除对应索引位置/范围的元素（索引/范围必须有效）  
        - (void)removeObjectAtIndex:(NSUInteger)index;  
        - (void)removeObjectsAtIndexes:(NSIndexSet *)indexes;  
        - (void)removeObjectsInRange:(NSRange)range;  
        //有则删除  
        - (void)removeObject:(id)anObject;  
        - (void)removeObject:(id)anObject inRange:(NSRange)range;  
        - (void)removeObjectsInArray:(NSArray *)otherArray;  
        - (void)removeAllObjects;  

