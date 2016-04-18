---
layout: post
title: "解耦神器 —— 统跳协议和Rewrite引擎"
date: 2016-04-18
author: "Asingers"
subtitle: "统跳协议学习 一"
categories: iOS
tags:
   - iOS
---
**题记：天猫App长大了，已经长成了流量以千万计规模的App，当下至少有10个团队在直接维护天猫App。在App长大，团队扩充的过程中解耦是一个永恒的话题，而界面解耦又是App架构的重中之重。**

统跳协议是天猫App统一跳转协议，主要负责天猫App界面之间的串联，也就是界面跳转服务。Rewrite引擎是与之配合的一套URL重写引擎，可以通过配置实现重写规则动态化。

## 历史上的今天

统跳协议的前身是一套叫做internal的协议，internal要重点解决的问题是在WebView和推送通知中如何跳转到指定的界面，进一步在任何动态场景下如何跳转到指定界面。在这样的思路下，internal中定义了多种协议格式，如：
`tmall://tmallclient/?{"action":""}`
`internal:url=`
`link:url=`
`tmall://mobile.tmall.com/page/`
几乎每一种场景都有一种格式的协议与之对应。在具体操作过程中这些协议都以URL表现出来。不难看出，这套协议最大的问题在于协议格式异构化严重，且不符合W3C的URL标准。随着App规模的扩大，场景日趋复杂，界面越来越多，这套协议的弊端也日益显露。

而在天猫App开始从百万级冲击千万级的时候，我们认识到一套格式统一，符合标准，规则简洁的协议非常必要。这套协议的任务也绝不是解决固定场景跳转，而是完全托管整个App的跳转工作，从而实现全App界面解耦和跳转动态化。因此，我们重新设计了界面协议，形成了当下这套规范——**统跳协议**。配合统跳协议，为了解决更多细节问题和跨平台问题，我们还设计了**Rewrite引擎**与之配合。

## 统跳协议

统跳协议设计之初就保留了很强的可扩展性，为接下来更丰富的场景预留了能力。上文讲到了统跳协议在界面跳转中作用，而事实上界面跳转仅仅是这套方案的一个典型场景，一个最佳实践。界面跳转在统跳协议的框架中被认为成一个服务，而跳转到哪一个界面则是由服务内部实现决定的。

![统跳协议服务注册](http://gtms02.alicdn.com/tps/i2/TB1DAewKFXXXXXwXFXX4HBiMpXX-790-494.png)

### 注册一个服务

服务通过声明URL的方式注册到统跳协议中，这个声明发生在服务所属模块内部的一个配置文件中，而这个配置文件被注册到统跳协议里。也就是说，整个App中的每一个模块都要注册一个配置文件到统跳协议，统跳协议在初始化过程中会遍历配置文件列表，逐一加载这些模块配置，根据配置信息把一个一个的模块服务注册到协议中。

![服务注册](http://gtms03.alicdn.com/tps/i3/TB1HWSnKFXXXXXmXVXX2s1YKFXX-680-734.png)

统跳协议要求调用服务的URL必须是符合W3C URL标准的，服务注册使用的URL只能包括host和path两部分，其中host是必须的，path则可选。当统跳协议接收一个跳转请求的URL后，先根据该URL的host和path两部分作为条件查找已注册的服务，再初始化对应服务，把URL交给服务实例执行后续操作。

### 如何实现服务

统跳协议声明了一个服务接口，这个接口中只有一个方法，服务必须由该接口实现而来。每一个服务可以通过实现接口中声明的方法，使用参数中传递来的完整URL，参数列表和调用发起者指针，执行具体业务逻辑。

例如分享服务，以iOS为例：实现了`TMShareUrlHandler`服务。


    @interfaceTMShareUrlHandler:NSObject<AliAppURLHandler>@end




    @implementationTMShareUrlHandler#pragma mark - URL调用分享组件-(id)handleUrl:(NSURL*)urlwithTarget:(id)targetwithParams:(id)params{// 省略代码详情returnnil;}@end



在分享模块的配置文件中声明该服务的URL为`sharekit.tm/doShare`。

这份配置文件在分享模块里：

![声明分享服务](http://gtms04.alicdn.com/tps/i4/TB1FSerKFXXXXa2XFXXwWjx4FXX-858-126.png)

分享模块的配置文件`sharekit_bundle.plist`也注册到统跳协议中。

这份配置文件在统跳协议模块里：

![注册分享模块](http://gtms01.alicdn.com/tps/i1/TB1p2KIKFXXXXcDXXXXyLW6.VXX-876-156.png)

### 统跳协议如何处理界面跳转

界面跳转是统跳协议的初衷，也是统跳协议最重要的任务。因此在统跳协议服务注册机制中，为界面服务注册做了更精细的定制开发。

上文提到跳转服务是一个单一服务，而界面则成百上千，所以在界面注册和服务注册中出现了冲突。本着降低开发成本的原则，我们又希望把同一个模块中界面注册和服务注册放在一起。所以在统跳协议中做了如下订制：

- 
默认注册跳转服务

跳转服务是默认存在的，在统跳协议初始化过程中这个服务就已经初始化了。

- 
给界面注册提供特殊的标记

上文中可以看到在注册分享服务的配置中`object`字段是服务的类名，若界面注册也按照这个规则，那么界面的类就会被认为成一个服务，在调用过程中必然会出现错误。因此我们约定，界面注册需要在类名前加`#`标示。

![界面注册](http://gtms03.alicdn.com/tps/i3/TB1P0GGKFXXXXXBXpXX6KVdUFXX-944-158.png)



如此一来，在统跳协议初始化过程中，默认加载跳转服务。当调用发生，解析URL查找到的对应对象带有`#`，则认为这是一个界面，则初始化这个对象，但不对其调用处理URL的方法，而是托管给已注册的跳转服务。跳转服务则根据URL和初始化的界面对象执行跳转服务。

## Rewrite

Rewrite引擎的思路来源于Web容器中的Rewrite机制，主要解决天猫App中URL平台展现一致性的问题。

天猫App中所有界面都是通过URL来标示的，然而标示Native界面的URL全部都建立在Native规范下，无法和其他平台对应起来，而Rewrite引擎通过重写URL来实现平台一致性。

例如：商品详情页面在PC Web的URL是`https://detail.tmall.com/item.htm`，在Mobile Web则是`https://detail.m.tmall.com/item.htm`，在Native声明的是`tmall://page.tm/itemDetail`。三者各不相同。PC Web和Mobile Web可以通过判断浏览器的UA识别环境，从而通过跳转实现一致性，也就是说在手机浏览器访问PC Web的URL，会通过一次302转到Mobile Web的URL。而Native App的环境具有一定的特殊性，Native界面则无法通过类似302这样的跳转来实现无感知切换，而Rewrite引擎就是来解决这个问题的。首先，无论是Native还是Web，在天猫App中他们两两之间的跳转都被统跳协议托管，而统跳协议在执行跳转操作之前会把原始URL放入Rewrite引擎中做一次Rewrite操作。这样一来，Rewrite引擎就根据配置规则，把原始URL转换成适用于天猫App的目标URL，实现了URL表现平台一致性。

### 原理

Rewrite引擎的原理非常简单，模拟Web容器（Apache/Nginx等）的Rewrite配置，根据配置把传入的原始URL进行重写，返回重写后的目标URL，交给统跳协议处理。

配置是通过正则表达式描述的Rewrite规则列表，这份列表通过猫客的配置中心实现动态更新。

### Rewrite规则

- 每条Rewrite规则中有三个字段：模式串，转换串和标记位
- 模式串：即正则表达式，用于匹配原始URL
- 转换串：即需要被转换成目标URL的描述
- 标记位：以西文逗号分隔的`标记位`，包括表示匹配则终止的`l`，需要进行店铺域名查询的`s`等


- Rewrite规则按行整理，并自上而下按顺序逐行匹配


### 转换模板中的保留字

- 
变量

变量由变量标示符和变量名组合而成，如：$0，$#1，2，$host，query，$#fragment等。变量被用在转换串中描述转换后的目标URL中的值。

- 变量标示符：$，$$和$#
- $：原变量的值
- $$：对原变量做URL Encode
- $#：自动识别编码，对原变量做URL Decode
- $$$：自动识别编码，对原变量做URL Decode，再以UTF8做URL Encode


- 变量名：数字（从0开始），枚举（scheme，host，port，path，query，fragment，shopid）
- 数字：0 - 表示整个URL，1~n - 表示正则中使用圆括号取出的参数
- 枚举：scheme，host，port，path，query，fragment表示标准URL中的相应部分；shopid表示对个性店铺域名查询后得到的shopid




- 
标记位

即上述规则中的标记位



### Rewrite引擎查询流程

1. 取出规则列表中的首条规则
2. 以模式串为模板对原始URL做匹配，并得到模式串定义的参数表
3. 若匹配成功则继续进行，否则进入下一条规则，从**2**开始进行下一轮匹配
4. 查看该条规则是否包含`s`标记位，若包含，则使用原始串做一次个性域名的查询
5. 使用1的结果和重写串对原始URL进行重写操作，得到目标URL
6. 查看该条规则是否包含`l`标记位：
- 若包含，则结束匹配，返回目标URL
- 若不包含，则把目标URL赋值给原始URL，并进入下一条规则，从**2**开始下一轮匹配


7. 直到最后一条规则结束，返回目标URL


### 举例

上述提到过商品详情页的例子，在Rewrite配置中就体现为：
模式串转换串标记位^(?:https?:)?\/\/detail(?:.m)?.tmall.com\/?item.htm\?(.*)tmall://page.tm/itemDetail?$1l
在这条规则的保护下，PC Web和Mobile Web下的商品详情URL在天猫App中都会被拦截到Native商品详情页面，可以带来最好的用户体验。也就是说，在日常的运营工作中，不需要关注一个商品在某个平台内部需要以什么样的URL来投放，只需要投放一个主要的URL格式。这个URL在天猫App内部会被Rewrite引擎重写为Native界面声明的URL，进行展示。

## 统跳和Rewrite在双11中的表现

统跳协议和Rewrite引擎在刚刚过去的双11期间，在全链路界面降级方案和会场上下线中发挥了重要作用。

### 全链路界面降级

上文中提到了天猫App中全部界面都声明了自己的一个`tmall://`协议的Native URL，但在业务逻辑上使用的不是这个Native URL，而是和Web保持一直的`http://`协议的URL，在统跳协议中会调用Rewrite引擎对这个`http://`URL进行重写后再做展现。

为了保证整个天猫App的可用性，我们在配置列表中预先定义了一系列Rewrite规则，用于拦截这些URL。一旦发现Native逻辑出现异常，将快速上线预定义的规则，从而在Rewrite引擎把`http://`URL重写成Native URL之前拦截，直接返回`http://`URL，实现Native界面到Mobile Web界面的降级。

### Native会场上下线

双11会场是双11活动期间曝光率最高的页面，也是对体验要求最高的界面。因此，我们在天猫App双11版本中对重要会场做了Native化，以提升用户体验。而Native会场展现依赖会场数据，且开启时间严格控制在双11当日的24小时内。

在这样的要求下，我们配置了`http://`协议的会场URL到Native会场URL的Rewrite规则，并在双11开始时上线，结束时下线，实现了双11当日天猫App的会场Native化。

## 结语

统跳协议在设计过程中预留了很好的扩展能力，所以在界面解耦之外还承担了更多的服务调用功能。Rewrite引擎为实现平台一致性而设计，而在实际应用过程中又挖掘出更多的场景和功能。

在统跳协议和Rewrite引擎接下来的发展过程中，将更注重Android和iOS双平台的高度一致性，并尝试开放更多API，让所有人一起挖掘这套方案的潜力。

> 原文来自[查看原文](http://pingguohe.net/2015/11/24/Navigator-and-Rewrite.html)  




