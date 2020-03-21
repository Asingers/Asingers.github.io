---
layout: post
title: remote: Repository not found
subtitle: 明明有的仓库地址找不到了？？？
categories: tips
header-mask: 0.7
tags: git

---

最近clone repo 的时候遇到一个怪事：


```
remote: Repository not found.
fatal: repository 'https://github.com/xxx/xxx.git/' not found
```
第一反应：这TM不应该呀。搜了搜，还真有相关的解答：

```
git credential-manager uninstall
git credential-manager install
```

这的好像有点眼熟，git 密码管理相关的，这操作看起来好像像那么回事，然而无效。又一想，这是我一个private 的repo，难道？那也应该提示我输入密码才对呀。我的为甚么没有提示。又一想，git 是可以读取用户保存在Keychains 里的密码的，这也是为什么使用SourceTree 有时候会提示你输入电脑密码，没错就是在读取密码。
> [Caching your GitHub password in Git](https://help.github.com/en/github/using-git/caching-your-github-password-in-git)

又联想到，最新我有登录我另一个github 帐号的操作，会不会有可能没有区分到不同的账户？这时候去🔑 Keychain 里看了一下github 相关的账户，果然有多个，那么我删除无关的试试，果然。

可以了。