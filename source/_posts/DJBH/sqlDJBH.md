---
title: SqlDJBH
#src: ../title.jpg
permalink: sqlDJBH
date: 2019-03-26 22:58:58
updated: 2019-04-18 16:19:00
categories: 等级保护
tags: 
    - oracle
    - mysql
---
{% blockquote %}
数据库等级保护笔记
{% endblockquote %}
<!--more-->
# 查看版本
```sql oracle
select * from v$version;
select version from v$instance;
select version FROM Product_component_version Where SUBSTR(PRODUCT,1,6)='Oracle';
```

```sql mysql
select version();
status;
```

# 身份鉴别
Oracle默认提供了一个密码策略的sql执行文件`utlpwdmg.sql`
```sql oracle
# Linux
$ORACLE_HOME/rdbms/admin/utlpwdmg.sql #默认位置
# Windows
# windows路径位置可以通过搜索来获得。
C:\app\Administrator\product\11.2.0\dbhome_1\RDBMS\ADMIN #默认位置

# 查看用户状态及profile文件
select username,account_status,profile from dba_users;
# 只查看激活的用户
select username,account_status,profile from dba_users where account_status='OPEN'; 

# profile设置
select profile,resource_name,limit from dba_profiles where resource_type='PASSWORD';
select profile,resource_name,limit from dba_profiles where resource_type='PASSWORD' and profile='DEFAULT'; #只查看默认profile
# 参数
PASSWORD_VERIFY_FUNCTION #复杂度设置，一般为null。
PASSWORD_LIFE_TIME       #口令有效期，默认为180。
PASSWORD_REUSE_TIME      #使用过口令多久可以再次被使用，默认为UNLIMITED。
PASSWORD_REUSE_MAX       #口令被修改后原有口令被修改多少次才允许被重新使用，默认为UNLIMITED。
PASSWORD_GRACE_TIME      #表示用户口令使用时间超过其生命周期后，可以延续使用的天数，并且可延续时间内登录会有相应口令即将过期的提示，默认为7天。
FAILED_LOGIN_ATTEMPTS    #登录失败次数，默认为10。
PASSWORD_LOCK_TIME       #账户锁定时间，默认为1。
```

```sql mysql
#查看密码策略
show VARIABLES like "%password%"; 
show variables like 'validate_password%'; 

#查看用户
select user,host,password from mysql.user;

#查看插件信息
show plugins;
```

# 访问控制
```sql oracle
select * from V$PWFILE_USERS;  #查看哪些用户有sysdba或sysoper系统权限
select * from user_sys_privs;  #查看当前用户系统权限
select * from dba_sys_privs;   #查看所有用户系统权限
select * from role_sys_privs;  #查看角色(只能查看登陆用户拥有的角色)所包含的权限
select * from dba_role_privs;  #查看所有用户的角色
select * from user_role_privs; #查看当前用户的角色
select * from dba_roles;       #查看所有角色
select * from dba_tab_privs;   #查看所有用户对象权限
select * from user_tab_privs;  #查看当前用户对象权限
select * from all_tab_privs;   #查看当前用户可查看用户的对象权限
#查看当前用户的缺省表空间
select username,default_tablespace from user_users; 
```

```sql mysql
show grants for root@localhost; #查看指定用户的权限
show grants;                    #查看当前用户的权限
```

# 安全审计
```sql oracle
show parameter audit;                #查看日志
show parameter audit_sys_operations; #启用对sys用户的审计
show parameter audit_trail;          #日志存放位置，若为os，则保存在系统中，windows系统在事件查看器中。
select * from v$logfile;             #日志文件路径
select * from v$sql;                 #查看最近所作的操作
select * fromv $sqlarea;             #查看最近所作的操作
```

```sql mysql
show variables like '%log%'; #查看日志
show variables like '%audit%';
# 参数
错误日志：  -log-err  
查询日志：  -log  
慢查询日志：-log-slow-queries  
更新日志：  -log-update  
二进制日志：-log-bin  
```

# 资源控制
## 地址限制
```sql mysql
select host,user,password from mysql.user where host='%';
```

## 超时锁定
```sql oracle
# 查看idle_time设置，默认为UNLIMITED。
select resource_name,limit from dba_profiles where profile='DEFAULT' and resource_type='KERNEL';
```

```sql mysql
show global variables like '%timeout%';
# 参数
interactive_timeout #服务器关闭交互式连接前等待活动的秒数，默认值为28800。
wait_timeout        #服务器关闭非交互连接之前等待活动的秒数，默认值为28800。
```

# 其他
```sql oracle
select * from user_users; #查看当前用户
select * from dba_users;  #查看数据库的所有用户
select * from all_users;  #列出当前用户可见的数据库的所有用户
```

```sql mysql
select user();            #查看当前用户
select database();        #查看当前数据库
show database;            #查看所有数据库
show tables;              #显示当前数据库的所有表
select * from mysql.user; #查看所有用户
show plugins;             #查看插件信息
```

<br>
<br>