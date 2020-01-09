---
title: LinuxDJBH
date: 2019-01-09 20:32:24
updated: 2019-12-11 20:32:24
permalink: LinuxDJBH
categories: 等级保护
tags: 
    - Linux
---
{% blockquote %}
Linux等级保护2.0笔记
{% endblockquote %}
<!--more-->
# 查看版本
```bash
# 查看Linux 内核版本
cat /proc/version
uname –a
uname –r
# 查看Linux 版本：
lsb_release –a
cat /etc/redhat-release
cat /etc/centos-release
rpm -q redhat-release
rpm -q centos-release
cat /etc/release #Solaris
```

# RedHat/CentOS
## 身份鉴别
### 密码策略
- /etc/login.defs

```bash 
# 默认参数
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_MIN_LEN    5
PASS_WARN_AGE   7

# 推荐参数
PASS_MAX_DAYS   90 #密码有效期/天
PASS_MIN_DAYS   1  #更换密码的最小天数
PASS_MIN_LEN    8  #密码最小长度,实际无效，需要通过system-auth文件来进行控制
PASS_WARN_AGE   7  #密码失效前多少天开始警告
```

- /etc/pam.d/system-auth

```bash
# 参数
password requisite pam_cracklib.so minlen=8 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
# minlen=8 最小密码长度为8
# ucredit=-1 至少需要有一个大写字母
# lcredit=-1 至少需要有一个小写字母
# dcredit=-1 至少需要有一个数字
# ocredit=-1 至少需要一个除字母数字以外的其他字符
```

```bash 查看单个用户的密码策略
chage -l username

# 参数
Last password change                                    : Dec 12, 2018 # 上次修改时间
Password expires                                        : never        # 密码过期时间
Password inactive                                       : never        # 密码失效时间
Account expires                                         : never        # 帐户过期时间
Minimum number of days between password change          : 0            # 两次改变密码之间相距的最小天数
Maximum number of days between password change          : 99999        # 两次改变密码之间相距的最大天数
Number of days of warning before password expires       : 7            # 在密码过期之前警告的天数 
```

```bash 口令更改时间
# 通过shadow文件计算
cat /etc/shadow
date -u -d "1970-01-01 UTC $((16749 * 86400 )) seconds"
```

### 登录失败
```bash 
# 文件
/etc/pam.d/system-auth
/etc/pam.d/login
/etc/pam.d/sshd

# 参数
auth required pam_tally2.so onerr=fail deny=5 unlock_time=100 even_deny_root root_unlock_time=100
# even_deny_root 也限制root用户；
# deny 设置普通用户和root用户连续错误登陆的最大次数，超过最大次数，则锁定该用户
# unlock_time 设定普通用户锁定后，多少时间后解锁，单位是秒；
# root_unlock_time 设定root用户锁定后，多少时间后解锁，单位是秒；

account required /lib/security/pam tally.so deny=X no_magic_root reset
```

### 登录超时
- /etc/profile

```bash 
TMOUT=120
```

- /etc/ssh/ssh_config

```bash
ConnectTimeout=120
```

### 远程管理协议
```bash 
service sshd status
service telnetd status
```

### 双因子身份鉴别

## 访问控制

## 安全审计
### 审计进程
```bash 系统日志服务
service syslog status #(CentOS6或redhat6以上为rsyslog)
service auditd status
#或
service --status-all | grep running
#查看进程
PS -ef | grep syslog
ps -ef | grep auditd
```

### 日志配置文件
```bash 
/etc/audit/audit.rules
```

- /etc/rsyslog.conf、/etc/syslog.conf

```bash rsyslog.conf文件
# 将info或更高级别的消息送到messages，除了mail、authpriv、cron以外。
# 其中*是通配符；none表示不对任何级别的信息进行记录。
*.info;mail.none;authpriv.none;cron.none                /var/log/messages

# 将授权和安全相关的消息记录到secure文件中
authpriv.*                                              /var/log/secure

# 由mail子系统产生的消息记录到maillog中.
mail.*                                                  -/var/log/maillog

# cron守护进程相关的信息记录到cron文件中
cron.*                                                  /var/log/cron

# 最高级别消息发送给所有在线的用户
*.emerg                                                 *

# 网络消息子系统.重要情况
uucp,news.crit                                          /var/log/spooler

# 将和系统启动相关的信息记录到boot.log
local7.*                                                /var/log/boot.log
```

- 日志设施有：
>auth(security), authpriv: 授权和安全相关的消息
kern: 来自Linux内核的消息
mail: 由mail子系统产生的消息
cron: cron守护进程相关的信息
daemon: 守护进程产生的信息
news: 网络消息子系统
lpr: 打印相关的日志信息
user: 用户进程相关的信息
local0 to local7: 保留，本地使用

- 日志级别有(升序)：
>debug：包含详细的开发情报的信息，通常只在调试一个程序时使用。
info：情报信息，正常的系统消息，比如骚扰报告，带宽数据等，不需要处理。
notice：不是错误情况，也不需要立即处理。
warning：警告信息，不是错误，比如系统磁盘使用了85%等。
err：错误，不是非常紧急，在一定时间内修复即可。
crit：重要情况，如硬盘错误，备用连接丢失。
alert：应该被立即改正的问题，如系统数据库被破坏，ISP连接丢失。
emerg：紧急情况，需要立即通知技术人员。

[如何对rsyslog进行配置](https://www.mtyun.com/library/how-to-config-rsyslog)

## 入侵防范
### 系统服务
```bash
service --status-all
ps -ef
setup #通过图形界面记录系统开启的网络服务
chkconfig --list #查看各运行级别下系统开启的服务，命令行为3,5为图形化
who –r #查看系统当前运行级别
```

### 端口
```bash
netstat -lntp #查看tcp端口
netstat -lnup #查看udp端口
netstat -ano  #早期Linux
```

### 地址限制
- /etc/hosts.allow、/etc/hosts.deny

```bash 参数
sshd:192.168.1.0/24:allow
sshd:*:deny
```

- /etc/ssh/sshd_config

```bash 参数
AllowUsers root test@192.168.1.1
DenyUsers root test@192.168.1.1
```

- iptables

```bash 命令
iptables -n -L
```

### 补丁包
```
rpm -qa grep patch
```


####################

# Solaris
## 身份鉴别
### 密码策略
- /etc/default/passwd

```bash 参数
MINWEEKS=1   # 密码可以被改变的最小时段
MAXWEEKS=13  # 密码最大的生存周期
WARNWEEKS=4  # 设定密码到期提醒天数
PASSLENGTH=8 # 设定密码长度
MINALPHA=2   # 口令必须包含字母的最小数目
MINDIFF=1    # 口令必须包含唯一字符的最小数目
```

### 登录失败
```bash 
/etc/security/policy.conf
/etc/default/login

LOCK_AFTER_RETRIES=YES
RETRIES=5
```

### 远程协议
```bash Solaris
svcs | grep ssh
svcs | grep telnet
```

## 访问控制


## 安全审计


```bash Solaris
/etc/default/login
SYSLOG=YES #审计进程运行正常

svcs |grep auditd #audit服务
```

### 审计内容


```bash Solaris
/etc/syslog.conf 
*.info;mail.none;news.none;authpriv.none;cron.none /var/log/messages

/etc/security/audit_control
#应设置需审计的事件类，如：
#audit_control file
dir: /var/audit
flags:1o
minfree:20
naflags:lo
*审计事件类型至少应包括：lo、fw、fm、fd。
```

### 日志文件
```bash RadHat/CentOS
/var/log/messages
/var/log/secure
/var/log/audit/audit.log
```

```bash Solaris
/var/adm/messages；
/var/adm/wtmpx；
/var/adm/utmpx；
```

## 入侵防范
### 端口 
```bash 
netstat -lntp #查看tcp端口
netstat -lnup #查看udp端口
netstat -ano  #早期Linux
```

### 服务
```bash Solaris
svcs -a
```

### 补丁
```bash Solaris
cat /etc/release #系统版本、安全补丁安装
```

### 地址限制
```bash Solaris
/etc/default/login
# 参数
CONSOLE=/dev/console
```



# ~~系统资源~~(2.0无要求)
```bash RadHat/CentOS
# 文件
/etc/security/limits.conf
# 1、fsize 用户创建的文件大小限制；
# 2、core 生成的core文件大小的限制；
# 3、cpu 用户进程可用cpu的限定值；
# 4、data 进程数据段大小的限定值；
# 5、stack 进程堆栈短大小的限定值；
# 6、rss 进程常驻内存段的限定值；
# 7、nofiles 进程中打开文件的最大数量。
```

```bash Solaris
ulimit -a
# 参数
time(seconds)
file(blocks)
data(kbytes)
stack(kbytes)
memory(kbytes)
coredump(blocks)
nofiles(descriptors)
```

<br>
<br>