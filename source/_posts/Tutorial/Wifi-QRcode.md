---
title: 手机扫码连接wifi
date: 2018-07-03 09:53:53
updated: 2018-07-03 09:53:53
permalink: Wifi-QRcode
categories: 教程
tags:
  - IOS
  - Android
  - 二维码
---
{% blockquote %}
发现有微信扫码连接Wifi这种操作，于是折腾一下二维码
{% endblockquote %}
<!--more-->

# 微信扫一扫
用二维码解码工具看看内容是什么↓
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Wifi-QRcode/2018-07-03-11-24-03.png)
可以看出是一个网址链接，用浏览器打开
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Wifi-QRcode/2018-07-03-11-28-45.png)
提示需要用微信打开，微信扫码可以连接wifi
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Wifi-QRcode/2018-07-03-11-33-23.png)
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Wifi-QRcode/2018-07-03-11-33-32.png)
然后去安利了一波方法(百度就有)：[微信连wif申请、配置、使用全版教程](https://jingyan.baidu.com/article/3a2f7c2edc3e1026afd61197.html)

<br>
# 手机相机扫一扫
参考： [Android、IOS手机扫描家庭二维码连接WiFi](https://www.chenyudong.com/archives/wifi-qrcode-in-home-android-and-ios.html)
```
WIFI:T:WPA/WPA2;S:wifiname;P:wifipasswd;;
```
说明：
`WIFI`表示这个是一个连接WiFi的协议
`S`表示后面是WiFi的 SSID，`wifiname`也就是 WiFi 的名称
`P`表示后面是WiFi的密码，`wifipasswd`是 WiFi 的密码
`T`表示后面是密码的加密方式，`WPA/WPA2`大部分都是这个加密方式，也有的路由器是使用`WPA`。
`H`表示这个WiFi是否是隐藏的，直接打开 WiFi 扫不到这个信号。苹果还不支持隐藏模式

然后将这段文字生成二维码即可，在线二维码生成地址：[在线生成二维码(QR码)](http://tool.oschina.net/qr)
## IOS
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Wifi-QRcode/2018-07-03-11-53-32.jpg)

## 小米
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Wifi-QRcode/2018-07-03-12-35-04.png)

使用限制：
支持系统自带扫码功能的手机，无论是相机，系统浏览器，还是系统自带扫一扫都可以
苹果IOS11及以上版本相机支持二维码扫码

# 总结
微信扫码设置麻烦，需要商户版本的公众账号，普通个人的公众账号不能使用，但是用户体验一级棒；
手机扫码设置方便，但是用户体验较差，需要支持系统二维码扫码的手机才可以。

<br>
<br>