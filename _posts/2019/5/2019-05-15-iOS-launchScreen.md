---
layout: post
title: Do you really understand iOS Launch Screen
subtitle: 你真的用好Launch Screen 了吗
categories: iOS
header-mask: 0.7
tags: 
    - iOS

---


# Do you really understand iOS Launch Screen?
最近在使用一些app 的过程中发现，它们的Launch Screen 与众不同，这才突然想来Launch Screen 这个在iOS 中不起眼但却非常重要的一环。现在市面上大多app 的 Launch Screen 的做法无外乎这几种：

* 图片、视频广告，例如新浪微博
* 留白页面加上Logo
* 动画效果

事实上，这些骚操作增加了app 的启动时间，也就是首屏映入用户眼帘的时间。那为什么大家都在这么做呢？甚至一些还有一些 诸如 “如何减少app 启动时间的文章” 有点本末倒置的感觉。最重要的原因还是想要把Launch Screen 作为优先级较高的展示位。一是可以宣传公司形象，展示app 个性，二就是一些app 拥有大量用户，流量即收益，这也就是一些在Launch Screen 展示后添加广告的做法。

### 那么，Apple 建议开发者怎么做呢？

Apple 是这么说的：

> A launch screen appears instantly when your app starts up. The launch screen is quickly replaced with the first screen of your app, giving the impression that your app is fast and responsive. The launch screen isn’t an opportunity for artistic expression. It’s solely intended to enhance the perception of your app as quick to launch and immediately ready for use.   

很明显，快速放在了首位，作为一个过渡页面，它的作用只是为了让这个家在过程看起来快和顺理成章。

![](/images/post/2019-05-16_19.29.18.jpg)

在iOS 自带的app 中都可以找到这个设计理念的存在，例如上图中的Safari，Launch Screen 其实是简化了的第一页，即在内容尚未加载完成时的过渡页面。这也就是为什么iOS 能持续给人非常迅速顺滑的体验。感官这个东西真的很重要。

这样做的好处就是，给人一种非常迅速的感觉，启动后就能看到即将加载的内容，非常好的体验。

> Design a launch screen that’s nearly identical to the first screen of your app. If you include elements that look different when the app finishes launching, people can experience an unpleasant flash between the launch screen and the first screen of the app.  
>   
> Avoid including text on your launch screen. Because launch screens are static, any displayed text won’t be localized.  
>   
> Downplay launch. People are likely to switch apps frequently, so design a launch screen that doesn’t draw attention to the app launching experience.  
>   
> Don’t advertise. The launch screen isn’t a branding opportunity. Don’t design an entry experience that looks like a splash screen or an "About" window. Don’t include logos or other branding elements unless they’re a static part of your app’s first screen.  

除此之外，在eBay、SnapChat、Instagram、Google Photo 等app 中都发现这种做法。在我使用过的国内app 中尚未发现这种优雅的做法。




