---
title: 家用网络设计
date: 2019-02-04 13:59:50
updated: 2019-02-04 13:59:50
src: //dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/title.jpg
permalink: jiayongwangluosheji
categories: Network
tags:
    - 家庭网络
    - 拓扑图
---
{% blockquote %}
前段时间忙于搬家，现在闲下来把新家的网络设计情况理了理
{% endblockquote %}
<!--more-->
# 拓扑图
![拓扑结构](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/家用网络.jpg)

# 准备阶段
## 带宽
>- 电信300M带宽
- 光纤入户

## 设备
>- [x] 天翼网关1台
- [x] 交换机1台
- [x] 集线器1个
- [x] 无线路由2台
- [x] 无线信号放大器3个
- [x] 摄像头4个
- [x] 摄像头控制设备1台

## 其它
>- [x] 网线标签贴若干
- [x] 网线若干
- [x] T型台卡扫码牌1个

# 网络配置
## 天翼网关
>- 考虑办理电信送的设备性能情况，不开启网关的wifi功能
- 设置dhcp地址池
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/2019-02-04-14-47-23.png)

## master网络
>- 自动获取地址
- 使用华为智能家居APP管理CD28和无线信号放大器

## guest网络
>- guest路由器版本较老，为了方便管理设置静态地址
![地址设置](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/2019-02-04-14-40-37.png)
- 开启外网web登录
![允许web访问与ping连接](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/2019-02-04-14-43-11.png)

## 监控
>- 所有监控设备IP地址调整至200以后
- 监控设备通过集线器相连
- 使用监控APP管理监控视频

# 访问控制
## 核心部分
>- 交换机开启MTU VLAN，使内网设备间无法互相访问（监控设备通过集线器进行通讯）
![MTU VLAN](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/2019-02-04-15-02-58.png)

## guest网络
>- guest网络由于集线器物理限制至100mbps（其实因为tv线的关系网线的8个铜线被剪成两根每根4铜线了，接交换机速度也跑不上去。。。）
>- 启用站点限制功能，使guest网络内用户无法访问核心网络
![站点限制](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/2019-02-04-15-10-21.png)

# 其它
- 为所有网线贴上标签方便标识
![网线标签](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/2019-02-04-15-16-03.jpg)
- 设置扫码连接wifi，[设置方法](/2018/07/03/Tutorial/Wifi-QRcode/)
![guest网络](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/jiayongwangluo/2019-02-04-15-23-57.jpg)

<br>
<br>