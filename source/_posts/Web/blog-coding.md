---
title: 记一次部署迁移
date: 2018-07-06 14:01:53
updated: 2018-07-06 14:01:53
permalink: blog-coding
categories: Web
tags: 
    - 开发
    - coding
---
{% blockquote %}
大佬推荐把博客部署在coding上，毕竟github在国外访问比较慢
{% endblockquote %}
<!--more-->

参考[玩转hexo-3-部署](http://blog.lujingtao.com/2017/11/19/hexo-deploy/)
# 创建项目
## 注册并新建项目
官网链接：[coding](https://coding.net/)
![新建项目](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-26-07.png)
![基本配置](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-28-08.png)
## 分支管理
![选择分支](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-35-16.png)
![新建分支](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-36-59.png)
![创建分支](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-38-06.png)
![修改默认分支](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-41-02.png)
![选择新建的分支](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-41-26.png)
## 启动page服务
![选择page服务](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-43-34.png)
![选择master分支](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-45-24.png)
<br>
# 通过git完成部署
## 修改部署路径
将之前的github注释掉，添加coding的deploy
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
#deploy:
  #type: git
  #repository: git@github.com:YuanLiChenAi/YuanLiChenAi.github.io.git
  #branch: master
deploy:
  - type: git
    repo: git@git.coding.net:YuanLiChenAi/YuanLiChenAi.git # 后面为:你的账号/刚才创建的仓库名.git
    branch: master
  - type: git
    repo: git@git.coding.net:YuanLiChenAi/YuanLiChenAi.git # 后面为:你的账号/刚才创建的仓库名.git
    branch: src
    extend_dirs: /
    ignore_hidden: false
    ignore_pattern:
        public: .
        .git: .
```
coding有两个分支，master用来部署网站，src用来备份网站源文件
效果如下：
![master保存静态html](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-55-55.png)
![src分支保存源文件](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog-coding/2018-07-06-14-56-36.png)
<br>
{% blockquote 特别注意%}
因为我是从github切换到coding的，所以目录下中含有`.git`隐藏目录
如果直接部署，则会报错。所以要在博客目录中用`rm -rf .git`命令来删除这个目录
删除之后就可以顺利部署了，而且不用再用`git push ...`备份了
{% endblockquote %}

<br>
<br>