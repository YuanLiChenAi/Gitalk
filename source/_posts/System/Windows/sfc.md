---
title: sfc
date: 2018-03-22 19:27:03
updated: 2018-03-22 19:27:03
permalink: sfc
categories: System
tags:
  - 系统命令
  - Windows
---
{% blockquote %}
今天浏览博客的时候被人植入了广告
{% endblockquote %}
<!--more-->

忘记截图保存了，不过广告的图片外链地址还在记录里，广告被放在了我博客的右边
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/sfc/anlian.png)
但是我的博客是静态的怎么会被植入广告，开始我是用Chrome访问的，然后我换成了360和Firefox访问的时候也都有。
最后我在别人的电脑浏览了我的博客，发现广告终于没有了。
想来应该是我电脑的问题，然后就用360杀一下呗
结果病毒没杀出来，把我的工具杀出来了
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/sfc/gongju.png)
好吧，三个浏览器都被改了，想想应该是系统被修改了，那就修复下系统吧
那么Windows下就可以使用sfc这个命令
这个命令必须是管理员才能执行的
```bash
PS C:\Users\zxw> sfc

为了使用 sfc 工具，你必须作为管理员运行控制台会话。
PS C:\Users\zxw>
```
```bash
PS C:\WINDOWS\system32> sfc /?

Microsoft (R) Windows (R) Resource Checker 6.0 版
版权所有 (C) Microsoft Corporation。保留所有权利。

扫描所有保护的系统文件的完整性，并使用正确的 Microsoft 版本替换
不正确的版本。

SFC [/SCANNOW] [/VERIFYONLY] [/SCANFILE=<file>] [/VERIFYFILE=<file>]
    [/OFFWINDIR=<offline windows directory> /OFFBOOTDIR=<offline boot directory>]

/SCANNOW        扫描所有保护的系统文件的完整性，并尽可能修复
                有问题的文件。
/VERIFYONLY     扫描所有保护的系统文件的完整性。不会执行修复
                操作。
/SCANFILE       扫描引用的文件的完整性，如果找到问题，则修复文件。
                指定完整路径 <file>
/VERIFYFILE     验证带有完整路径 <file> 的文件的完整性。
                不会执行修复操作。
/OFFBOOTDIR     对于脱机修复，指定脱机启动目录的位置
/OFFWINDIR      对于脱机修复，指定脱机 Windows 目录的位置

示例:

        sfc /SCANNOW
        sfc /VERIFYFILE=c:\windows\system32\kernel32.dll
        sfc /SCANFILE=d:\windows\system32\kernel32.dll /OFFBOOTDIR=d:\ /OFFWINDIR=d:\windows
        sfc /VERIFYONLY
PS C:\WINDOWS\system32>
```
使用sfc /SCANNOW后广告就消失了

<br>
<br>
