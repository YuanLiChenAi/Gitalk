---
title: WindowsDJBH
date: 2018-06-14 13:52:11
updated: 2019-12-25 16:00:00
permalink: WindowsDJBH
categories: 等级保护
tags: 
    - Windows
---
{% blockquote %}
Windows等级保护2.0笔记
{% endblockquote %}
<!--more-->
# 查看版本
```bash
systeminfo
```

# 身份鉴别
## 密码策略
- 本地安全策略

```bash 命令行打开
secpol.msc
```

开始→程序→管理工具→本地安全策略→安全设置→帐号策略→密码策略
```bash
# 默认值
密码必须符合复杂性要求：已禁用
密码长度最小值：0个字符
密码最短使用期限：0天
密码最长使用期限：42天
强制密码历史：0个记住的密码
用可还原的加密来储存密码：已禁用
# 推荐值
密码必须符合复杂性要求：已启用
密码长度最小值：8个字符
密码最短使用期限：1天
密码最长使用期限：90天
强制密码历史：5个记住的密码
用可还原的加密来储存密码：已禁用
```
>需在计算机管理→本地用户和组→用户 每个账户选择中取消勾选**密码永不过期**

- 查看账户信息

```bash
net user administrator
```

## 登录失败
开始→程序→管理工具→本地安全策略→安全设置→帐号策略→账户锁定策略
```bash
# 默认值
账户锁定时间：不适用
账户锁定阈值：0次无效登陆
重置账户锁定计数器：不适用
# 推荐值
账户锁定时间：30分钟
账户锁定阈值：5次无效登陆
重置账户锁定计数器：30分钟
```

## 登录超时
- 屏幕保护程序
需要密码保护，时间不超过30分钟。

- 远程桌面会话设置
    - Windows xp/Vista/2008/2008R2
管理工具→Terminal Servers-终端服务配置/终端服务配置，选择RDP-tcp查看属性，选择会话
        - - [x] 替代用户设置
空闲会话限制：10分钟
        - - [x] 替代用户设置
            - [ ] 从会话断开
            - [x] 结束会话

    - Windows 2012以上
打开**本地组策略编辑器**
```bash
gpedit.msc 
```
    管理模板→windows组件→远程桌面服务→会话时间限制
**活动但空闲的远程桌面服务会话的时间限制** 设置为10分钟
**达到时间限制终止会话** 设置为启用

## 远程桌面
- Windows xp/Vista/2008/2008R2
管理工具→Terminal Servers-终端服务配置/终端服务配置，选择RDP-tcp查看属性
```bash
安全层：协商
加密级别：客户端兼容
仅允许运行使用网络级别的身份验证的远程桌面的计算机连接：
```
- Windows 2012以上
注册表：计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp
```bash
SecurityLayer #安全层
0-RDPSecurity Layer
1-Negotiate
2-SSL(TLS1.0)

MinEncryptionLevel #加密级别
1-Low
2-ClientCompatible
3-High
4-FIPSCompliant

UserAuthentication
0-未勾选
1-勾选
```

```bash 推荐值
安全层：SSL
机密级别：客户端兼容/高/符合FIPS标准
仅允许运行使用网络级别的身份验证的远程桌面的计算机连接：勾选
证书：使用默认SSL证书/使用CA机构签发的证书
```

## 双因子身份鉴别

# 访问控制
```bash 用户账户和组
lusrmgr.msc
```

# 安全审计
## 审核策略
控制面板→管理工具→本地安全策略→本地策略→审核策略
```txt 推荐值
审核策略更改：成功
审核登录事件：成功，失败
审核对象访问：成功
审核过程追踪：成功
审核目录服务访问：失败
审核特权使用：成功
审核系统事件：成功
审核帐户登录事件：成功，失败
审核帐户管理：成功
```

## 日志内容
计算机管理→事件查看器

# 入侵防范

## 服务
```bash 命令行
services.msc
```

```txt 可能的多余服务
Print Spooler、Remote Registry、server、TCP\IP NetBIOS Helper、Task Scheduler
打印服务、远程注册表、文件共享、主机解析服务、计划任务
```

## 端口
```bash
netstat -ano | more
netstat -anb #端口以及执行文件，需管理员权限
netstat -ano #端口以及进程
netstat -ano | findstr 135
netstat -ano | findstr 139
netstat -ano | findstr 445
```

## 默认共享：
- 查看默认共享

```bash 查看默认共享命令
net share
```

```bash 参数
C:\Documents and Settings\Administrator>net share

共享名       资源                            注释

----------------------------------------------------------
IPC$                                         远程 IPC
ADMIN$       C:\WINDOWS                      远程管理
D$           D:\                             默认共享
C$           C:\                             默认共享
命令成功完成。
```

- 删除共享，重启后失效

在`计算机管理→系统工具→共享文件夹→共享`中删除
```bash 命令行
net share C$ /del
net share D$ /del
net share IPC$ /del
net share ADMIN$ /del
```

- 永久生效需修改注册表：

关闭C\$、D\$、E\$一类的共享：`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\lanmanserver\parameters`，`AutoShareServer`设置为0。
关闭ADMIN$共享：在同样的分支下，将右侧窗口中的`AutoShareWKs`设置为0。

<br>
<br>
