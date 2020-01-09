---
title: Google Hacking
date: 2018-03-22 12:40:34
updated: 2018-03-22 12:40:34
permalink: GoogleHacking
categories: 信息安全
tags: 
    - Google
    - 信息收集
---
{% blockquote %}
你会用Google嘛？
{% endblockquote %}
<!--more-->

先看图，这图是从知乎上拿下来的
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/GoogleHacking/GoogleHacking.jpg)

# 基本符号
{% blockquote %}
-keyword #强制结果不要出现此关键字
\*keyword\* #模糊搜索，强制结果包含此关键字
"keyword" #强制搜索结果出现此关键字
{% endblockquote %}

# 高级操作符
{% blockquote %}
site:
搜索指定的网页内容,可以用来搜索子域名、跟此域名相关的内容,例如：site:zhihu.com

filetype:
搜索指定文件类型,例如："web安全" filetype:pdf,搜索跟安全书籍相关的pdf文件

ext:
与filetype等价

inurl:
搜索url网址存在特定关键字的网页,可以用来搜寻有注入点的网站,例如：inurl:/admin/login.php,搜索网址中有"/admin/login.php"网页

intitle:
搜索标题存在特定关键字的网页,例如：intitle:后台登陆,搜索网页标题是“后台登陆”的网页

allinurl:
类似inurl:,但是可指定多个字符，不能与其他操作符混合使用，可单独使用

link:
搜索链接到所输入URL的页面，该操作符不需要关键字，不能与其他操作符及关键字混用

allintitle:
和intitle类似，能接多个关键字，但是不能与其他操作符混合使用，可单独使用

intext:
搜索正文存在特定关键字的网页,例如：intext:知乎

allintext:
使用方法和intext类似，能接多个关键字，能与其他操作符混合使用，可单独使用

cache:
输入URL，搜索特定页面的缓存快照，即使目标页面发生变动甚至不存在了，依然可以看到它的副本

define:
搜索输入关键词或关键词组的定义来源链接,例如搜索:define:script,将返回关于script的定义，该操作符不能与其他操作符及关键字混用。
{% endblockquote %}

# 注意点
操作符之间要有空格
引号为英文的双引号

# 搭配使用
在freebuf中搜索关于"黑客"的PDF文件
```
"黑客" site:freebuf.com filetype:pdf
```
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/GoogleHacking/heike.png)

<br>
<br>