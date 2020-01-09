---
title: 无线交换机
date: 2018-04-21 18:34:29
updated: 2018-04-21 18:34:29
permalink: DCN-DCWS
categories: Network
tags:
  - 路由交换技术
  - WirelessSwitch
  - Cisco
---
{% blockquote %}
信息安全管理大赛AC+AP部分，
{% endblockquote %}
<!--more-->
# 部分拓扑说明
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/DCN-DCWS/1.png)
基于dhcp option 43发现AP，所以需要先配置dhcp服务，
```bash
service dhcp #开启服务
ip dhcp excluded-address 192.168.2.1 192.168.2.2 #排除地址
ip dhcp pool vlan2
network-address 192.168.2.0 255.255.255.0
default-router 192.168.2.1
dns-server 114.114.114.114

#与AP相连的接口必须划分到对应vlan，这里是vlan 2
```
<br>
# 发现AP并注册
```bash
#IP十进制转16进制
192.168.2.2
1100 0000.1010 1000.0000 0010.0000 0010
C0A80202

#开启option43服务
ip dhcp pool ap
option 43 hex 0104C0A80202 #0104固定值，后面是16进制IP
option 60 ascii udhcp 1.18.2 #固定值，1.18.2是AP版本，详见配置手册

#配置无线部分
wireless
no auto-ip-assign #关闭IP自动查找
enable #开启
no discovery vlan-list 1 #不查找vlan1，默认配置查找vlan1
discovery vlan-list 2 #查找vlan2
static-ip  192.168.2.2 #静态AC管理IP
show wireless

DCWS(config)#show wireless ap failure status

    MAC Address
 (*) Peer Managed   IP Address                              Last Failure Type           Age
------------------ --------------------------------------- ------------------------ ----------------
 00-03-0f-78-e2-40 192.168.2.3                             No Database Entry        0d:00:00:15

#将AP注册到数据库
wireless
DCWS(config-wireless)#ap database 00-03-0f-78-e2-40
DCWS(config-ap)#exit
DCWS(config-wireless)#ap authentication none
DCWS(config-wireless)#show wireless ap status

    MAC Address                                                            Configuration
 (*) Peer Managed  IP Address                              Profile Status     Status           Age
------------------ --------------------------------------- ------- ------- ------------- --------------
 00-03-0f-78-e2-40 192.168.2.3                                     Failed  Not Config    0d:00:00:58

Total Access Points............................ 1
DCWS(config-wireless)#
```
<br>
# 无线网络配置
```bash
#network配置
DCWS(config-wireless)#network 2
DCWS(config-network)#security mode wpa-personal //认证模式wpa
DCWS(config-network)#ssid DCN
DCWS(config-network)#vlan 2
DCWS(config-network)#wpa key chinaskill //认证密码
DCWS(config-wireless)#network 3
DCWS(config-network)#security mode none //不认证
DCWS(config-network)#ssid guest
DCWS(config-network)#vlan 2

#配置AP的配置文件
DCWS(config-wireless)#ap profile 2 //配置AP 1配置
DCWS(config-ap-profile)#radio 2 //选择工作频率，1位2.4，位5
DCWS(config-ap-profile-radio)#enable //打开
DCWS(config-ap-profile-radio)#vap 1 //一个vap对应一个network
DCWS(config-ap-profile-vap)#enable
DCWS(config-ap-profile-vap)#network 2
DCWS(config-ap-profile-radio)#vap 2
DCWS(config-ap-profile-vap)#enable
DCWS(config-ap-profile-vap)#network 3

#下发配置
DCWS#wireless ap profile apply 1
All configurations will be send to the aps associated to this profile and associated clients on these aps will be disconnected.
Are you sure you want to apply the profile configuration? [Y/N] y
AP Profile apply is in progress.
DCWS#

#注意：network1、profile1、vap0都有默认配置，尽量从第二个开始
```

<br>
<br>
