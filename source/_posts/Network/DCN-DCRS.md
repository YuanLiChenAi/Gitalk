---
title: 交换机安全加固
date: 2018-04-21 12:20:12
updated: 2018-04-21 12:20:12
permalink: DCN-DCRS
categories: Network
tags:
  - 路由交换技术
  - Switch
  - Cisco
---
{% blockquote %}
最近要准备信息安全管理大赛，本文梳理下交换机加固的知识，比赛是基于神州数码（DCN）的设备，命令行是思科类型
{% endblockquote %}
<!--more-->

# 身份验证
## 用户创建
```bash
username test privilege 15 password 0 test #用户名、管理级别（15权限最大）密码
authentication line vty login local #设置vty用户登陆本地验证
enable password test #进入配置模式设置密码
```
## SSH
```bash
ssh-server enable #开启ssh服务
ssh-server timeout 300 #ssh登录超时，单位秒
ssh-server max-connection 5 #ssh登陆最大数量
ssh-server host-key create rsa #ssh秘钥，基于RSA算法
```
## web管理
```bash
ip http server #开启http服务，关闭前面加no
authentication line web login local #设置web用户登陆本地验证
#https
ip http secure-ciphersuite des-cbc-sha #设置加密方式
ip http secure-port 1025 #设置访问端口，1025-65535
ip http secure-server #开启https，必须在最后
```
## 802.1x认证
```bash
radius-server authentication host 10.103.117.7 #配置认证服务器
radius-server accounting host 10.103.117.7  #配置计费服务器
radius-server key 123   #配置认证服务器的密钥
aaa enable   #开启AAA认证
aaa-accounting enable  #开启AAA计费功能
dot1x enable  #开启802.1x认证
dot1x user free-resource 10.103.117.0 255.255.255.0 #设置认证前可访问资源

interface Ethernet 0/0/1   #进入端口
dot1x enable   #开启端口的1X认证
dot1x port-method userbased advanced #设置认证方式为userbased
dot1x port-control auto  #设置授权状态为auto
```
<br>
# 日志记录
## 网络日志审计
```bash
#将指定接口的流量镜像到流量审计服务器
monitor session 1 source inter ethernet 1/0/x both #防火墙的接口
monitor session 1 destination interface ethernet  1/0/x  #日志的接口
show monitor
```
## 交换机常规日志
```bash
info-center enable #开启日志记录
info-center loghost 192.168.1.1 facility local7 channel 9 #将log发送给日志服务器
```
<br>
# 二层防护
## 端口安全
```bash
mac-address-learning cpu-control #必须先全局开启mac地址学习
interface ethernet 1/0/2 #进入接口接口
switchport port-security #开启本接口的端口安全
switchport port-security maximum 5 #mac地址最大学习数量5
switchport port-security mac-address sticky #mac地址自动学习
switchport port-security violation restrict recovery 300 #当有违反规则的mac地址数据包通过限制接口，并在300秒后恢复，
#除此之外还有，shutdown、protect、recovery
switchport port-security aging time 1 #端口老化时间，用于忘记已经动态记住的mac地址
switchport port-security mac-address 2F-FF-FF-FF-FF-2F #手动绑定mac地址
```
## 私有vlan特性
详见ppt最后：[https://wenku.baidu.com/view/b180f202bed5b9f3f90f1c99.html](https://wenku.baidu.com/view/b180f202bed5b9f3f90f1c99.html)
```bash
#在DCRS交换机上配置私有VLAN特性阻止PC-3发起ARP DOS/DDOS渗透测试，并将DCRS交换机该配置信息截图。
#DCRS交换机该配置信息：
#Flag：
vlan 20 #PC-1连入的VLAN
private-vlan isolated
vlan 30 #PC-3连入的VLAN
private-vlan isolated
vlan 10 #DCST服务器场景连入的VLAN
private-vlan primary
private-vlan association 20;30

#私有vlan注意事项
#私有vlan配置的注意点
  #只有不包含任何以太网端口的vlan才能被设置Private VLAN
  #只有设置了关联关系的private vlan才能将Access类型的以太网端口设置为客户端
  #普通VLAN若被设置成Private VLAN后，会自动将所属以太网端口清空
#配置关联的注意点
  #只有Primary类型的VLAN才能设置Private VLAN关联关系
```
## 网关保护：ARP Guard
```bash
Interface Ethernet1/0/10 #特定接口，不支持vlan接口
arp-guard ip 192.168.10.1 #开启网关保护
```
## MAC地址访问控制列表，类似于ip access-list
```bash
mac-access-list extended name #创建mac地址访问控制列表，表名为“name”
deny host-source-mac 00-ff-51-be-ad-32 host-destination-mac 00-ff-51-be-ad-32 #拒绝源mac地址为”00-ff-51-be-ad-32的主机访问目的地址为“00-ff-51-be-ad-32”的主机
permit any-source-mac any-destination-mac #允许所有源主机访问目的主机
#在接口上启用
Interface Ethernet1/0/10
mac access-group name in
```
## 防ARP扫描
```bash
anti-arpscan enable #启动防ARP扫描功能
anti-arpscan recovery enable #开启自定恢复
anti-arpscan recovery time 3600 #配置自动恢复时间
anti-arpscan trust ip 192.168.10.1 255.255.255.0  #配置信任IP，非必要

Interface Ethernet1/0/2 #与主机相连的接口
anti-arpscan trust port #配置信任接口

interface ethernet 1/0/3 #与交换机相连的接口
anti-arpscan trust supertrust-port #超级信任端口，对端交换机也许开启功能并配置接口
```
## Arp local proxy
```bash
#功能：某种实际的应用环境中，为了防止 ARP 的欺骗，要求汇聚层的交换机实现local arp proxy功能，
#限制 ARP 报文在同一 vlan 内的转发，从而引导数据流量通过交换机进行 L3转发。
interface vlan 10
ip local proxy-arp
```
## ARP动态监测
```bash
#需要和dhcp结合使用
ip arp inspection vlan 1 #开启arp检测的vlan号
interface e1/0/18
ip arp inspection trust #配置信任接口

interface vlan 30
ip arp dynamic maximum 50 #VLAN接口的ARP阀值为50
```
## 环路检测
```bash
#配置端口环路检测
loopback-detection interval-time 35 15

int ethernet 1/0/1
loopback-detection special-vlan 1-3
loopback-detection control [shutdown,block]

#如果使用 block 控制，必须全局启动 mstp，并且配置生成树实例与 vlan 的对应关系
spanning-tree
spanning-tree mst configuration
instance 1 vlan 1
instance 2 vlan 2
```
## 广播风暴抑制
```bash
interface ethernet 1/0/10 #vlan对应接口
storm-control broadcast 400 #速率400
storm-control unicast 400
storm-control multicast 400
```
<br>
# DCHP防护
## DHCP snooping
```bash
ip dhcp snooping enable #开启dhcp侦听
ip dhcp snooping binding enable #开启dhcp侦听绑定功能
ip dhcp snooping binding arp #绑定arp
ip dhcp snooping binding dot1x #绑定dot1x
ip dhcp snooping limit-rate <pps> #配置 DHCP snooping 报文转发速率

interface ethernet 1/0/11 #进入接口
ip dhcp snooping trust #配置接口为信任项
interface ethernet 1/0/1-10 #多个接口配置
ip dhcp snooping action shutdown #配置接口为非信任项
```
<br>
# 抗Ddos攻击
```bash
dosattack-check srcip-equal-dstip enable #防IP Spoofing攻击
dosattack-check ipv4-first-fragment enable #启用对第一片段IPv4包的检查
dosattack-check tcp-flags enable #防TCP非法标志攻击
dosattack-check srcport-equal-dstport enable #启用TCP/UDP L4端口的检查，防止端口欺骗
dosattack-check tcp-fragment enable #防TCP碎片攻击
dosattack-check icmp-attacking enable #防icmp碎片攻击
dosattack-check icmpV4-size 64 #icmp包大小限制64
dosattack-check tcp-segment 20 #TCP段大小限制
```

<br>
<br>