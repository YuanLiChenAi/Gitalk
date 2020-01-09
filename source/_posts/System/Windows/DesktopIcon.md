---
title: Windows桌面图标样式修改
src: //dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/DesktopIcon/title.jpg #图片
permalink: DesktopIcon
date: 2019-08-28 17:11:04
updated: 2019-08-28 17:11:04
categories: System
tags:
  - Windows
  - 桌面图标
---
{% blockquote %}
更新了windows 1903后，发现桌面图标变得异常
{% endblockquote %}
<!--more-->
情况类似下图，图标之间间隙特别大
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/DesktopIcon/2019-08-29-21-38-48.png)
检查注册表发现系统更新之后桌面图标样式参数变化，IconSpacing(图标间距)和IconVerticalSpacing(图标垂直间距)参数变太大了，修改为正常值后就恢复正常了。
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/DesktopIcon/2019-08-29-22-12-01.png)

```text
# 路劲
计算机\HKEY_CURRENT_USER\Control Panel\Desktop\WindowMetrics

# 参数
BorderWidth 边框宽度
CaptionHeight 标题栏高度
CaptionWidth 标题栏宽度
IconSpacing 图标间距
IconTitleWrap 图标标题是否自动换行，1即是
IconVerticalSpacing 图标垂直间距
MenuHeight 菜单高度
MenuWidth 菜单宽度
MinAnimate 最小动画，1即是
PaddedBorderWidth 填充边框宽度
ScrollHeight 滚动高度
ScrollWidth 滚动宽度
Shell Icon Size 改变图标大小
SmCaptionHeight 调色板标题高度
SmCaptionWidth 调色板标题宽度
```

>参数单位为像素，设置后注销或重启后生效

<br>
<br>