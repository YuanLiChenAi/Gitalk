---
title: windows用户文件迁移
date: 2018-09-25 20:10:39
updated: 2018-09-25 20:10:39
permalink: window-user
categories: System
tags:
  - Windows
---
{% blockquote %}
今天把域账户文件夹手贱删除了，而且没备份，然后就登不回去，提示账户无法登陆。
{% endblockquote %}
<!--more-->
![无法登陆到你的账户](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/window-user/2018-09-25.jpg)
原本我是想删除原默认域账户文件，然后把本地账户文件夹名重命名，省的我在配置一次。结果凉凉，然后按照提示注销，重新登陆后仍然没用，还是这样。
尝试多种方法，发现以下情况：
{% blockquote %}
- 登陆域账户，与域账号同名的文件夹并没有被使用
- 用户文件夹下出现TEMP文件夹，这才是我当前的用户文件夹，不过注销后就自动删除
- 尝试删除并重新添加域账户无效，问题还在
{% endblockquote %}
于是先解决TEMP的问题，有时候因为账户配置文件出现问题，导致域用户登录电脑时，只能登录temp临时账户。通过删除注册表信息我们可以恢复域账户登录。
注册表地址：
```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList\S-1-5-21-793558431-3863633201-2293836228-1001
```
找到对应的注册表信息，并删除
![注册表](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/window-user/2018-09-25-20-38-47.png)
百度经验：[如何解决域用户登录Temp临时账户问题](https://jingyan.baidu.com/article/da1091fb6502b3027949d664.html)
写博客的时候看到另一个方法，[win无法登陆到你的账号怎么解决](https://jingyan.baidu.com/article/ca00d56c245854e99febcf57.html)