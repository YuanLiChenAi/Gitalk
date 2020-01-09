---
title: UnixDJBH
date: 2019-01-09 20:39:12
updated: 2019-02-03 20:39:12
permalink: UnixDJBH
categories: 等级保护
tags: 
    - Unix
---
{% blockquote %}
Unix等保笔记
{% endblockquote %}
<!--more-->
# 查看版本
```bash AIX
oslevel -r
```
# 身份鉴别
## 密码策略
```bash AIX
/etc/security/user
# 参数
maxage：8     #密码有效的星期数，指出两次修改密码之间的距离最长时间，在这个时间之前用户必须修改密码，该属性的值是一个正整数，范围为0-52.当为0时表示没有最短时间限制
minlen：8     #密码的最短长度，值的范围为0到8,0表示没有限制
minalpha：2   #新密码中含有字符的最少个数，这是一个正整数的值，他的范围为0到8，为0时表示没有最少字符限制
maxrepeats：3 #在新密码中相同支付出现的最大次数，0表示无限制，8表示没有最大数目的限制
mindiff：4    #指旧密码中的字符在新密码中出现的最小个数，该属性值是个正整数，范围为0到8.0表示没有限制
minother：2   #新密码中含有非字母字符的最少个数，这是一个正整数的值，他的范围是0到8,，为0表示无最少非字母字符数限制flags不为NOCHECK
histexpire：  #该用户在多个个星期内不能重复使用相同的密码，该属性是一个正整数，值的范围在0到260，若为0表示没有这个限制，推荐值为26（大约6个月）
histsize：    #该用户不能重复使用以前的密码（旧密码）的数，该属性的值是一个正整数，值的范围是0到50
```

```bash HP-UX
/etc/default/security
# 参数
MIN_PASSWORD_LENGTH=8 #最短密码长度
PASSWORD_HISTORY_DEPTH=5 #密码历史
PASSWORD_MIN_UPPER_CASE_CHARS=1 #最小大写字母个数
PASSWORD_MIN_LOWER_CASE_CHARS=1 #最小小写字母个数
PASSWORD_MIN_DIGIT_CHARS=1 #最小数字个数
PASSWORD_MIN_SPECIAL_CHARS=1 #最小特殊字符个数
PASSWORD_MAXDAYS=90 #最长有效期
PASSWORD_MINDAYS=14 #最短有效期

# 如果上述策略文件未配置，则查看是否开启了信任模式（trusted system），如果开启了，查看以下内容：
SAM-Auditing and Security-System Security Policies-password format policies
SAM-Auditing and Security-System Security Policies-password format policies-Maximum Password Length
SAM-Auditing and Security-System Security Policies-password Aging policies-password expiration time(days)
SAM-Auditing and Security-System Security Policies-password Aging policies-password life time(days)
```

## 口令更换时间
```bash
cat /etc/security/passwd
perl -le 'print scalar localtime 1200982154'
```

## 登录失败
```bash AIX
/etc/security/login.cfg
logindisable:5    #5次失败登录后锁定端口
logininterval:60  #在60秒内5次失败登录才锁定端口
loginreenable：30 #端口锁定30分钟后解锁

/etc/security/user
loginretries=5    #密码输错5次账户锁定,0表示不限制
```

```bash HP-UX
/etc/default/security
# 参数
AUTH_MAXTRIES

# 如果上述策略文件未配置，则查看是否开启了信任模式（trusted system），如果开启了，查看以下内容：
SAM-Auditing and Security-System Security Policies-General User Account policies-Unsunccessful Login Tries Allowed
```

## 远程协议

# 访问控制
## umask
```bash
# 默认值
022
# 推荐值
077
```

# 安全审计
## 审计进程
```bash 
ps -ef|grep syslog #syslog
```

```bash AIX
audit query #audit
```

```bash HP=UX
SAM-Auditing and Security-Audited users #打开对所有用户的审计
```

## 审计内容
```bash AIX
# 文件
audit query
/etc/syslog.conf
# 配置信息
kern    #由kernel产生的信息；
user    #由用户进行产生的信息；
mail    #邮件系统产生的信息；
daemon  #系统守护进程的信息；
auth    #由login，su，getty等进行身份认证时产生的信息；
syslog  #由syslogd自己内部产生的信息；
uucp    #UUCP系统信息；
cron    #cron和at工具信息；
emerg   #紧急，通常应广播到所有用户；
alert   #告警，当前状态必须立即进行纠正。例如，系统数据库崩溃；
crit    #关键状态的警告。例如，硬件故障；
err     #其他错误；
warning #警告；
notice  #注意；非错误状态的报告，但应特别处理；
info    #通报信息；
debug   #调试程序时的信息。
```

```bash HP-UX
SAM-Auditing and Security-Audited Event #打开对admin，login，modacccess，moddac事件的审计
/etc/syslog.conf
```

## 日志文件
```bash AIX
/var/adm/message
/var/adm/sulog 
/var/adm/wtmp 
/etc/utmp/security/faillogin 
/etc/security/lastlog
```

```bash HP-UX
/var/.audit/audfile1
/var/.audit/audfile2
/var/adm/syslog/*.log
```


# 入侵防范
## 端口 
```bash 
netstat -lntp #查看tcp端口
netstat -lnup #查看udp端口
netstat -ano  
```

## 服务
```bash AIX
lssrc -a #查看所有系统组服务
lssrc -a|grep active #查看所有启动的服务
```

## 组件
```bash
/etc/inetd.conf
```

## 补丁
```bash AIX
oslevel -r #查看操作系统当前 AIX 版本达到的最高的推荐维护包
oslevel -q #查看操作系统已知的版本号和名称
instfix -i | grep AIX_ML #查看打了哪些补丁
instfix -I | grep AIX_ML oslevel -r
instfix -ivk IY07276 #查看是否打了某个补丁
```

```bash HP-UX
swlist -l bundle
```

# 资源控制
## 地址限制
```bash
/etc/hosts.allow
/etc/hosts.deny
/etc/ssh/sshd_config
```

```bash HP-UX
cat /etc/securetty #console
cat /var/adm/inetd.sec #指定IP进行远程管理
```

## 超时锁定
```bash
# 文件
/etc/profile
/etc/ssh/ssh_config
# 值
TMOUT=120
ConnectTimeout=120
```

```bash HP-UX
System Security Policies-Terminal Security Policies-Login Timeout Value
```

## 系统资源
```bash 
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

```bash HP-UX
ulimit -a
```

<br>
<br>