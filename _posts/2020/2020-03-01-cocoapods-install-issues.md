---
layout: post
title: mkmf.rb can't find header files for ruby at...
description: macOS 10.14 ,Xcode 11,cocoapods install issues
categories: ios
tags: Xcode

---
由于 Xcode 11 路径的问题，导致可能 macOS 不是最新的 10.15 版本时候在执行`gem install cocoapods` 的时候报错。

`mkmf.rb can't find header files for ruby at /System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/lib/ruby/include/ruby.h`

请尝试以下步骤：
```
（1）sudo rm -rf /Library/Developer/CommandLineTools
（2）xcode-select --install
（3）sudo xcodebuild -license accept
```

问题主要是因为 Xcode 11 携带了 macOS 10.15 SDK，该 SDK 包含了 ruby 2.6 的头文件，但是对 macOS 10.14 系统的 ruby 2.3 却没有该文件，可以通过一下命令来验证问题

`ruby -rrbconfig -e 'puts RbConfig::CONFIG["rubyhdrdir"]'`

macOS 10.14 系统上，Xcode 11 版本安装的情况下会打印出这个不存在的路径

`/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/System/Library/Frameworks/Ruby.framework/Versions/`

Xcode 11 是安装在 macOS 10.14 SDK 上，在此路径/Library/Developer/CommandLineTools/SDKs/MacOS10.14.sdk 。但是它没有必要因为安装了旧的头文件而污染了系统目录。所以我们要改成，指定合适的 SDK 和 ruby 2.3 头文件：

`sudo xcode-select --switch /Library/Developer/CommandLineTools`

再来看下ruby 2.3的正确路径：

`ruby -rrbconfig -e 'puts RbConfig::CONFIG["rubyhdrdir"]'`

会输出一个正常的存在的路径：

`/Library/Developer/CommandLineTools/SDKs/MacOSX10.14.sdk/System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/include/ruby-2.3.0`

然后再次安装cocoapods 就可以了。

`gem install cocoapods`

当然这个🥚疼的步骤完全可以通过升级到最新的 macOS 😂
