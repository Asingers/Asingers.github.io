---
layout: post
title: iOS Localizations
subtitle: 项目本地化
categories: Mac
header-mask: 0.7
tags: 
    - iOS
    - Xcode

---

### Project->Info->Localizations
然后点击"+"，添加需要国际化/本地化的语言

### 基本信息本地化

选中Info.plist，按下键盘上的command + N，选择Strings File（iOS->Resource->Strings File）

	文件名字命名为InfoPlist
	
选中InfoPlist.strings，在Xcode的File inspection（Xcode右侧文件检查器）中点击Localize，目的是选择我们需要本地化的语言，点击Localize后，会弹出一个对话框，展开对话框列表，发现下拉列表所展示的语言正是我们在上面配置的需要国际化的语言，选择我们需要本地化的语言

接下来，我们分别用不同的语言给InfoPlist.strings下的文件设置对应的名字。如：
	
	CFBundleDisplayName = "Localizable App Name";
	
### 代码中字符串的本地化

同理和应用名称本地化一样，首先需要command + N，选择iOS -> Resource -> Strings File

	文件名必须命名为Localizable
	
然后我们只需要在Localizable.strings下对应的文件中，分别以Key-Value的形式，为代码中每一个需要本地化的字符串赋值，如：

	"Hello" = "你好"
	
然后代码中的字符串需要用

	NSLocalizedString(key, comment)
	
如

	NSLocalizedString("Hello", nil)
