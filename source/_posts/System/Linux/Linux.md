---
title: Linux基础
date: 2018-03-27 16:22:52
updated: 2018-03-27 16:22:52
permalink: Linux
categories: System
tags:
  - Linux
---
{% blockquote %}
Linux的一些基础命令和一些技巧
{% endblockquote %}
<!--more-->

# 常用基础命令
{% blockquote %}
ls　用来显示目标列表
cd　[path]　用来切换工作目录
pwd　以绝对路径的方式显示用户当前工作目录
man　[command]　查看Linux中的指令帮助、配置文件帮助和编程帮助等信息
apropos　[whatever]　在一些特定的包含系统命令的简短描述的数据库文件里查找关键字
echo　[string]　打印一行文本，参数“-e”可激活转义字符
cat　[file]　连接文件并打印到标准输出设备上
less　[file]　允许用户向前或向后浏览文字档案的内容
mv　[file1]　[file2]　用来对文件或目录重新命名，或者将文件从一个目录移到另一个目录中
cp　[file1]　[file2]　用来将一个或多个源文件或者目录复制到指定的目的文件或目录
rm　[file]　可以删除一个目录中的一个或多个文件或目录，也可以将某个目录及其下属的所有文件及其子目录均删除掉
ps　用于报告当前系统的进程状态
top　实时查看系统的整体运行情况
kill　杀死一个进程
ifconfig　查看或设置网络设备
ping　查看网络上的主机是否工作
netstat　显示网络连接、路由表和网络接口信息
nc(netcat)　建立TCP和UDP连接并监听
su　切换当前用户身份到其他用户身份
touch　[file]　创建新的空文件
mkdir　[dir]　创建目录
chmod　变更文件或目录的权限
chown　变更某个文件或目录的所有者和所属组
nano/vim/emacs　字符终端的文本编辑器
exit　退出shell
管道命令符　'|'　将一个命令的标准输出作为另一个命令的标准输入
{% endblockquote %}

# Bash快捷键
{% blockquote %}
Up(Down)　上（下）一条指令
Ctrl + c　终止当前进程
Ctrl + z　挂起当前进程，使用“fg”可唤醒
  fg [工作号]　将进程放到前台运行
  bg [工作号]　将进程放到后台运行
Ctrl + d　删除光标处的字符
Ctrl + l　清屏
Ctrl + a　移动到命令行首
Ctrl + e　移动到命令行尾
Ctrl + b　按单词后移（向左）
Ctrl + f　按单词前移（向右）
Ctrl + Shift + c　复制
Ctrl + Shift + v　粘贴
{% endblockquote %}

# 根目录结构
{% blockquote %}
/bin  、 /sbin  ：链接到  /usr/bin  ，存放 Linux 一些核心的二进制文件，其包含的命令可在 shell 上运行。
/boot  ：操作系统启动时要用到的程序。
/dev  ：包含了所有 Linux 系统中使用的外部设备。需要注意的是这里并不是存放外部设备的驱动程序，而是一个访问这些设备的端口。
/etc  ：存放系统管理时要用到的各种配置文件和子目录。
/etc/rc.d  ：存放 Linux 启动和关闭时要用到的脚本。
/home  ：普通用户的主目录。
/lib  、 /lib64  ：链接到  /usr/lib  ，存放系统及软件需要的动态链接共享库。
/mnt  ：这个目录让用户可以临时挂载其他的文件系统。
/proc  ：虚拟的目录，是系统内存的映射。可直接访问这个目录来获取系统信息。
/root  ：系统管理员的主目录。
/srv  ：存放一些服务启动之后需要提取的数据。
/sys  ：该目录下安装了一个文件系统 sysfs。该文件系统是内核设备树的一个直观反映。当一个内核对象被创建时，对应的文件和目录也在内核对象子系统中被创建。
/tmp  ：公用的临时文件存放目录。
/usr  ：应用程序和文件几乎都在这个目录下。
/usr/src  ：内核源代码的存放目录。
/var  ：存放了很多服务的日志信息。
{% endblockquote %}

# 字节序
目前计算机中采用两种字节存储机制：大端（Big-endian）和小端（Little-
endian）。
小端口诀: 高高低低 -> 高字节在高地址, 低字节在低地址
大端口诀: 高低低高 -> 高字节在低地址, 低字节在高地址

# 输入输出
使用命令的输出作为可执行文件的输入参数
```bash
$ ./vulnerable 'your_command_here'
$ ./vulnerable $(your_command_here)
```
使用命令作为输入
```bash
$ your_command_here | ./vulnerable
```
将命令行输出写入文件
```bash
$ your_command_here > filename
```
使用文件作为输入
```bash
$ ./vulnerable < filename
```

<br>
<br>