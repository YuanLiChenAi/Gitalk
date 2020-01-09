---
title: 设置文件右键快捷菜单
date: 2018-10-13 13:38:23
updated: 2018-10-13 13:38:23
permalink: RightClick
categories: System
tags:
    - 注册表
    - 右键菜单
    - Windows
---
{% blockquote %}
经常用到WinHex，但是每次需要打开winhex然后再打开文件过于繁琐，于是研究了添加右键快捷菜单设置方法
{% endblockquote %}
<!--more-->

# 文件右键快捷菜单设置
1. `Win+R` 输入 `regedit` 打开运行注册表,找到 `计算机\HKEY_CLASSES_ROOT\*\shell`项，右键新建项，名称`WinHex`(可随意)
![右键新建项](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/RightClick/2018-10-13-13-48-05.jpg)
2. 右键`WinHex`项，新建字符串值`Icon`，数据为`WinHex.exe`的文件路径，如下图
![新建字符串值](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/RightClick/2018-10-13-13-49-48.jpg)
3. 右键`WinHex`项，新建`Command`项，在右侧窗口的“默认”键值栏内输入程序所在的路径 如：`E:\学习\Tools\WinHex\WinHex.exe %1` 其中的%1表示要打开的文件参数。
![设置Command值](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/RightClick/2018-10-13-13-57-47.png)
4. 设置完成即可生效。
![效果图](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/RightClick/2018-10-13-14-00-39.png)

# 文件夹右键快捷菜单设置
设置方法与[文件右键快捷菜单](/https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/RightClick/2018/10/13/Notes/Right-click/#文件右键快捷菜单设置)相同，路径：`计算机\HKEY_CLASSES_ROOT\Directory\shell\git_shell`
![路径](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/RightClick/2018-10-13-14-05-58.png)

<br>
<br>