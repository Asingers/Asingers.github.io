---
layout: post
title: Download historical version ipa from the App Store
description: 从App Store 下载历史版本
categories: iOS
tags: App Store

---
由于某个特殊的需求我需要获取一个App 的某个历史版本，并且是可以正常使用的，也就是App Store 版本。过程还是比较顺利的，所以记录一下，如果你碰巧看到，希望对你有所帮助。当然抓取手机上的网络请求也是可以的，这里用iTunes 会更方便一些，并且.ipa 会直接存储在本地。*以下操作在Mac 上进行*

### 工具
* iTunes 
* Charles

iTunes 是为了进行网络请求，Charles 是为了抓取网络请求，并搞一些事情。注意：如果你的iTunes 是最新版本，那么已经不再内置 *应用* 菜单了，也就是说在iTunes 中看不到App Store 相关的内容（如果你知道我在说什么) 那么你需要先进行接下来的操作

### iTunes 降级

如果你的macOS 为10.13，请下载[官方可以部署App 的iTunes版本](https://support.apple.com/zh-cn/HT208079)即可。
如果你的为10.14，那么这个安装后将提示您的::系统不支持这个版本:: 那么你需要进行接下来的操作。

### 在macOS Mojave 中安装iTunes 12.6.X

1. 关闭SIP (系统完整性保护)
	1. 终端中执行 `csrutil status`  查看当前SIP 状态，可以看到是enabled 还是disabled
	2. 关闭：重启MAC，按住cmd+R直到屏幕上出现苹果的标志和进度条，进入Recovery模式
	3. 在工具栏找到实用工具，打开终端，输入：csrutil disable
	4. 重启mac
	5. 确认状态是否已经关闭
2. 使用脚本编辑器工具进行重装
	
#### 卸载现有的：
	

```
	
set question to display dialog "Delete iTtunes?" buttons {"Yes", "No"} default button 1
set answer to button returned of question
if answer is equal to "Yes" then
    do shell script "rm -rf /Applications/iTunes.app" with administrator privileges
    display dialog "iTunes was deleted" buttons {"Ok"}
    set theDMG to choose file with prompt "Please select iTunes 12.6 dmg file:" of type {"dmg"}
    do shell script "hdiutil mount " & quoted form of POSIX path of theDMG
    do shell script "pkgutil --expand /Volumes/iTunes/Install\\ iTunes.pkg ~/tmp"
    do shell script "sed -i '' 's/18A1/14F2511/g' ~/tmp/Distribution"
    do shell script "sed -i '' 's/gt/lt/g' ~/tmp/Distribution"
    do shell script "pkgutil --flatten ~/tmp ~/Desktop/iTunes.pkg"
    do shell script "hdiutil unmount /Volumes/iTunes/"
    do shell script "rm -rf ~/tmp"
end if
if answer is equal to "No" then
    display dialog "iTunes was not deleted" buttons {"Ok"}
    return
end if

set question to display dialog "Install iTtunes?" buttons {"Yes", "No"} default button 1
set answer to button returned of question
if answer is equal to "Yes" then
    do shell script "open ~/Desktop/iTunes.pkg"
    return
end if
if answer is equal to "No" then
    display dialog "Modified iTunes.pkg saved on desktop" buttons {"Ok"}
    return
end if
```

#### 安装12.6.x

```
set theAPP to choose file with prompt "Please select iTunes 12.6 app:" of type {"app"}
do shell script "sed -i '' 's/12.6.5/12.9.4/g' " & POSIX path of theAPP & "Contents/Info.plist" with administrator privileges
set question to display dialog "iTunes was patched. Open iTunes?" buttons {"Yes", "No"} default button 1
set answer to button returned of question
if answer is equal to "Yes" then
    do shell script "open " & POSIX path of theAPP
    return
end if
if answer is equal to "No" then
    display dialog "Modified iTunes saved on " & (POSIX path of theAPP as text) buttons {"Ok"}
    return
end if	
```

运行时如果 提示 iTunes Library.itl 错误,需要删除旧的数据库文件

```
sudo rm ~/Music/iTunes/iTunes\ Library.itl
```

### 抓取请求

如何使用Charles 抓取网络HTTPS 请求这里不再赘述，也不是本文的重点。

使用安装好的可用的iTunes 搜索并下载一个app ，在Charles 中会有大量的网络请求，我们需要关注的是包含pxx-buy.ituns.apple.com 关键字的这个网络请求，也就是购买时的相关信息。

#### 获取关键参数
在response 中找到*softwareVersionExternalIdentifiers* 这个Key ,value 为一个Array，包含了这个app 所有历史版本的标示，并且为升序排列。我们要做的事Copy 一个版本号数字备用，例如我要下载当前App Store 里最新版本的上一个版本，那么需要Copy 数组倒数第二个值。

#### 携带我们的参数重新调取网络请求

在这个网络请求处打一个断点，然后在Header 中修改关键参数，这个和Xcode 中的在断点处修改参数很相似。*appExtVrsId* 这是一个String ，并且修改值为上面我们拿到的版本号即可。OK，参数也该完了，接下来Execute 继续放开断点即可，那么这次的网络请求的参数就已经被我们修改为我们想要的了。

至此，一个历史版本的.ipa 就已经下载完了。总结一下就是修改了网络请求的参数，由于App Store 中app 有版本历史这个特性所以我们可以根据对应的的版本号从而拿到对应的.ipa

𝑰 𝒍𝒐𝒗𝒆 𝑨𝒑𝒑𝒍𝒆 .
𝑷𝒆𝒂𝒄𝒆!

<iframe
  src="https://carbon.now.sh/embed/?bg=rgba(74%2C144%2C226%2C1)&t=seti&wt=none&l=text%2Fx-objectivec&ds=true&dsyoff=20px&dsblur=68px&wc=true&wa=true&pv=56px&ph=56px&ln=false&fm=Hack&fs=14px&lh=133%25&si=false&es=2x&wm=false&code=%25F0%259D%2591%25B0%2520%25F0%259D%2592%258D%25F0%259D%2592%2590%25F0%259D%2592%2597%25F0%259D%2592%2586%2520%25F0%259D%2591%25A8%25F0%259D%2592%2591%25F0%259D%2592%2591%25F0%259D%2592%258D%25F0%259D%2592%2586.%2520%250A%25F0%259D%2591%25B7%25F0%259D%2592%2586%25F0%259D%2592%2582%25F0%259D%2592%2584%25F0%259D%2592%2586!%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520%2520"
  style="transform:scale(0.7); width:1024px; height:473px; border:0; overflow:hidden;"
  sandbox="allow-scripts allow-same-origin">
</iframe>

