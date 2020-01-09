---
title: 博客搭建笔记
date: 2018-03-20 10:24:20
updated: 2019-03-04 22:24:20
permalink: blog
repo: YuanLiChenAi | blog
categories: Web
tags: 
	- 开发
	- hexo
---
{% blockquote %}
梳理下blog搭建所涉及的知识，框架：Node.js+Git+Hexo+Makrdown
{% endblockquote %}
<!--more-->

# 前言
我的博客是基于node.js+git+Hexo+Markdown搭建的，代码托管在Github，域名从阿里云买的，映射到Github的IP。


# 环境安装
## 安装Node.js
在 Windows 环境下安装 Node.js 非常简单，仅须到[官网下载](https://nodejs.org/en/download/)安装文件并执行即可完成安装。安装无脑下一步即可。

## 安装Git
Git教程：[传送门](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
下载链接：https://git-scm.com/downloads
安装完成后鼠标右键菜单多了两个选项
![右键菜单栏](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/git.png)

### Git基本操作
用于blog备份，现在已经不需要了
```bash
# 新建一个仓库
git init
# 配置仓库
git config
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
# 将远程仓库复制到本地
git clone 项目地址
# 抓取远程仓库所有分支更新并合并到本地
git pull
# 将文件放到暂存区
git add 文件名/文件夹名
git add . 或者 git add --all # 全部内容
# 提交你的修改
git commit -m "注释"
# 远程仓库操作：增加/删除
git remote add 仓库名称 仓库地址
git remote rm 仓库名称
# 推送本地仓库更新到远程仓库
git push -u 远程名 本地分支
# 合并分支
git pull
```

#### Git提交远程库流程
将文件放入暂存区->提交修改->将本地更新推送到远程仓库

## Hexo
常用命令
```bash
npm install hexo-cli -g # 全局安装hexo全家桶
npm install # 安装基础依赖
hexo init blog # 初始化配置
hexo generate # 生成本地静态文件,作用不大，本地查看不需要生成public文件夹；部署时直接hexo d默认先生成public(public文件夹不存在时)
hexo server # 查看本地效果,开启服务，地址：localhost:4000
hexo deploy # 远程部署,推送内容
hexo new 文章名 # 创建新文章
```

### 安装部署插件
```bash
npm install hexo-deployer-git --save
```

#### 旧的部署配置
```m
 deploy:
   - type: git
     repo: git@git.coding.net:YuanLiChenAi/仓库名.git 
     branch: master
```
这是我参考的完整教程的博客 https://my.oschina.net/ryaneLee/blog/638440

#### 新的部署配置
配置代码比原方式多一个分支，可以将源文件备份在分支中。
以如下代码为例，需要提前在仓库创建分支，并把src分支设置为默认分支，不然`git clone`的内容是master的内容，也就是`hexo g`生成的public。
```m
 deploy:
   - type: git
     repo: git@git.coding.net:YuanLiChenAi/仓库名.git 
     branch: master
   - type: git
     repo: git@git.coding.net:YuanLiChenAi/仓库名.git 
     branch: src
     extend_dirs: /
     ignore_hidden: false
     ignore_pattern:
         public: .
         .git: .
```
具体的部署流程：[记一次部署迁移](/2018/07/06/Web/blog-coding/)
完整的配置流程：[玩转hexo - 1 - 蠢萌的序](http://blog.lujingtao.com/2017/11/07/hexo-new/)、[玩转hexo - 2 - 起步](http://blog.lujingtao.com/2017/11/07/hexo-init/)、[玩转hexo - 3 - 部署](http://blog.lujingtao.com/2017/11/19/hexo-deploy/)、[玩转hexo - 4 - 主题](http://blog.lujingtao.com/2017/11/20/hexo-theme/).

{% blockquote 注意事项%}
- `git clone`后需要将`.git`文件夹删除， 
- 安装hexo插件`npm install hexo --save`
- 第一次部署时会生成`.deploy_git`文件夹，在下次部署时需要删除该文件夹，不然会部署失败
{% endblockquote %}

# Makrdown编写
Markdown语法：http://xianbai.me/learn-md/article/about/readme.html

## ~~Atom~~
问题太多，已经不用了，看下面的[VScode](/2018/03/20/blog/#VScode)
### ~~安装Atom~~
下载安装Atom：https://atom.io/

### ~~插件安装~~
{% blockquote %}
markdown-preview-plus #增强预览，需要关闭自带的markdown-preview，与markdown-preview-enhanced冲突，已停用
atom-simplified-chinese-menu #中文包
language-markdown #代码高亮
markdown-preview-enhanced #可视化+同时滚动
markdown-table-editor #表格
markdown-image-paste #贴图，记得把_config.yml中的post_asset_folder设置为true，new文章会自动创建一个文件夹
markdown-themeable-pdf #pdf导出
pdf-view #pdf 查看
{% endblockquote %}

## VScode
### 安装VScode
下载地址：[https://code.visualstudio.com/](https://code.visualstudio.com/)

### 插件安装
{% blockquote %}
Markdown Preview Enhanced #增强预览
Paste Image #贴图，Ctrl+Alt+V
{% endblockquote %}
![预览按钮](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/2018-07-03-14-29-40.png)

# blog主题
我使用的主题是mellow
github地址：https://github.com/codefine/hexo-theme-mellow.git
预览地址：https://blog.lujingtao.com/

## 安装依赖
所需依赖：
```
hexo-generator-search #local search搜索
hexo-generator-topindex #文章置顶
hexo-helper-qrcode #二维码分享
hexo-renderer-less #less解析器
```
安装命令：
```
npm install --save hexo-generator-search hexo-generator-topindex hexo-helper-qrcode hexo-renderer-less
```

## 新建分类页
开启分类、标签页等，不仅需要在主题配置文件的menu中开启，还需要用命令行进行创建，具体方法：
先用hexo new page categories创建分类页源文件，然后在source/categories/index.md中修改layout: categories。

## 样式修改
### 自定义样式
路径：themes\mellow\source\css\_custom\custom.less

### 修改的_partial/less文件
路径：themes\mellow\source\css\_partial\
文章css：variable.less
代码css：highlight.less

## 增加more按钮
最新版本已加入该功能
路径：themes\mellow\layout\_partial\index-item.ejs
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/more1.png)
```html
    <a href="<%- url_for(post.path) %>" class="post-more waves-effect waves-button">
        <%- theme.excerpt_link %>
    </a>
```
主题的config中增加
```
excerpt_link: more      #更多按钮
```
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/more2.png)
<br>
## 链接为域名
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/link1.png)
在博客config文件里修改
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/link2.png)
<br>
## 搜索按钮
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/menu1.png)
```html
      <ul class="nav">
        <%
          var menuItem, isActive = function(item) {
              var pageUrl = url_for(page.path)
              return item.url === '/' ? pageUrl === url_for(item.url + 'index.html') : _.startsWith(pageUrl, url_for(item.url))
          };
          for (var i in theme.menu) {
            menuItem = theme.menu[i];
            if (menuItem.url) {
          %>
            <li class="<% if(isActive(menuItem)){ %> active<% } %>">
              <a href="<%- url_for(menuItem.url) %>" <% if(menuItem.target){ %>target="_blank"<% } %> >
                <i class="icon icon-lg icon-<%= menuItem.icon %>"></i>
                <%=(menuItem.text || _.startCase(i)) %>
              </a>
            </li>
        <% } else { %>
            <li>
              <a href="javascript:;" onclick="$('#site_search_btn').click();">
                <i class="icon icon-lg icon-<%= menuItem.icon %>"></i>
                <%=(menuItem.text || _.startCase(i)) %>
              </a>
            </li>
        <% } } %>
      </ul>
```
![](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/blog/menu2.png)
注意这个search和其他的不一样，不能有url这个属性

# 领养一个小姐姐
github地址：[https://github.com/EYHN/hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d)

## 安装
安装模块：
```bash
npm install --save hexo-helper-live2d
```

## 配置
在站点根目录的 _config.yml文件中添加配置.
示例:
```m
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: ./source/live2d_models/live2d-widget-model-koharu #模型位置及名称
  display:
    position: right
    width: 120
    height: 240
  mobile:
    show: true
```

## 模型
1.使用`npm install 模型的包名`来安装，下载的包在node_modules里面
2.将下载的包扔进配置中的位置`./source/live2d_models/`
模型预览：https://huaji8.top/post/live2d-plugin-2.0/
以下为所有模型
```
live2d-widget-model-chitose
live2d-widget-model-epsilon2_1
live2d-widget-model-gf
live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16
```

<br>
<br>