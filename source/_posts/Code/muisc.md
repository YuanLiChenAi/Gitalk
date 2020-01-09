---
title: markdown插入音乐
#src: ../title.jpg
date: 2019-03-03 22:27:31
updated: 2019-03-04 23:27:31
permalink: music
categories: Code
tags:
    - hexo
    - Aplay
    - MeingJS
---
{% meting "404465743" "netease" "song" %}
{% blockquote %}
想在博客里插入音乐，折腾了一下午，记录下各种方法
{% endblockquote %}
<!--more-->
<br>

# 音乐平台官方外链方式
```html iframe插件
<iframe 
    frameborder="no" border="0" marginwidth="0" 
    marginheight="0" width=330 height=86 
    src="//music.163.com/outchain/player?type=2&id=461544312&auto=0&height=66">
</iframe>
```
```html flash插件
<embed 
    src="//music.163.com/style/swf/widget.swf?sid=461544312&type=2&auto=0&width=320&height=66" 
    width="340" height="86"  allowNetworking="all">
</embed>
```
- 效果

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=461544312&auto=0&height=66"></iframe>

缺点:
- 之前有版权保护的歌曲可以通过修改id来播放，但是现在不行了，似乎被限制了。
- 没有滚动歌词等，功能过于简单


# MeingJS
# 加载相关JS文件
可以在文章开头添加，或者去修改主题
```html 
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer@1.7.0/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer@1.7.0/dist/APlayer.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/meting@1.1.0/dist/Meting.min.js"></script>
```

# 在文章中使用
```代码
<div class="aplayer" data-id="31356499" data-server="netease" data-type="song" data-mode="single"></div>
```

主要参数 | 值
------------ | ------------- 
data-id | 歌曲/专辑/歌单 ID
data-server | netease（网易云音乐）tencent（QQ音乐） xiami（虾米） kugou（酷狗）
data-type | song （单曲） album （专辑） playlist （歌单） search （搜索）
data-mode | random （随机） single （单曲） circulation （列表循环） order （列表）
data-autoplay | false（手动播放） true（自动播放）

- 效果

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer@1.7.0/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer@1.7.0/dist/APlayer.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/meting@1.1.0/dist/Meting.min.js"></script>
<div class="aplayer" data-id="31356499" data-server="netease" data-type="song" data-mode="single"></div>

缺点：
- 修改歌单歌曲并不会使外链变动
- 音乐云盘的歌曲（网易没有版权的歌曲）无法播放


# 使用Aplay插件
## 安装插件
Audio：[hexo-tag-aplayer](https://github.com/MoePlayer/hexo-tag-aplayer)
```bash 安装插件
npm install hexo-tag-aplayer
```
## 语法
```markdown 语法
{% aplayer title author url [picture_url, narrow, autoplay, width:xxx, lrc:xxx] %}
```
>- `title` : 曲目标题
- `author`: 曲目作者
- `url`: 音乐文件 URL 地址
- `picture_url`: (可选) 音乐对应的图片地址
- `narrow`: （可选）播放器袖珍风格
- `autoplay`: (可选) 自动播放，移动端浏览器暂时不支持此功能
- `width`:xxx: (可选) 播放器宽度 (默认: 100%)
- `lrc`:xxx: （可选）歌词文件 URL 地址

用来生成外链的地址：[刘志进实验室](https://music.liuzhijin.cn/)

## 歌词标签
插入歌词除了用文件，也可以用歌词标签
```hexo
{% aplayerlrc "title" "author" "url" "autoplay" %}
[00:00.00]lrc here
{% endaplayerlrc %}
```

## 播放列表
```
{% aplayerlist %}
{
    "narrow": false,                          // （可选）播放器袖珍风格
    "autoplay": true,                         // （可选) 自动播放，移动端浏览器暂时不支持此功能
    "mode": "random",                         // （可选）曲目循环类型，有 'random'（随机播放）, 'single' (单曲播放), 'circulation' (循环播放), 'order' (列表播放)， 默认：'circulation' 
    "showlrc": 3,                             // （可选）歌词显示配置项，可选项有：1,2,3
    "mutex": true,                            // （可选）该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停
    "theme": "#e6d0b2", |                       // （可选）播放器风格色彩设置，默认：#b7daff
    "preload": "metadata",                    // （可选）音乐文件预载入模式，可选项： 'none' 'metadata' 'auto', 默认: 'auto'
    "listmaxheight": "513px",                 // (可选) 该播放列表的最大长度
    "music": [
        {
            "title": "CoCo",
            "author": "Jeff Williams",
            "url": "caffeine.mp3",
            "pic": "caffeine.jpeg",
            "lrc": "caffeine.txt"
        },
        {
            "title": "アイロニ",
            "author": "鹿乃",
            "url": "irony.mp3",
            "pic": "irony.jpg"
        }
    ]
}
{% endaplayerlist %}
```
缺点：
- 歌曲信息需要自己去手动添加比较麻烦

# MeingJS 支持 (3.0 新功能)
MetingJS 是基于Meting API 的 APlayer 衍生播放器，引入 MetingJS 后，播放器将支持对于 QQ音乐、网易云音乐、虾米、酷狗、百度等平台的音乐播放。

在 Hexo 配置文件 _config.yml 中设置：
``` _config.yml
aplayer:
  meting: true
```
``` 代码
{% meting "391125700" "netease" "playlist" "autoplay" "mutex:false" "listmaxheight:340px" "preload:auto" "theme:#ad7a86"%}
```

选项 | 默认值 | 描述
:---: | :---: | --- 
id | 必须值 | 歌曲 id / 播放列表 id / 相册 id / 搜索关键字
server | 必须值 | 音乐平台: netease, tencent, kugou, xiami, baidu
type | 必须值 | song, playlist, album, search, artist
fixed | false | 开启固定模式
mini | false | 开启迷你模式
loop | all | 列表循环模式：all, one,none
order | list | 列表播放模式： list, random
volume | 0.7 | 播放器音量
lrctype | 0 | 歌词格式类型
listfolded | false | 指定音乐播放列表是否折叠
storagename | metingjs | LocalStorage 中存储播放器设定的键名
autoplay | true | 自动播放，移动端浏览器暂时不支持此功能
mutex | true | 该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停
listmaxheight | 340px | 播放列表的最大长度
preload | auto | 音乐文件预载入模式，可选项： none, metadata, auto
theme | #ad7a86 | 播放器风格色彩设置

- 效果

{% meting "391125700" "netease" "playlist" "mutex:false" "listmaxheight:340px" "preload:auto" "theme:#ad7a86"%}

缺点：
- 同样无法更新歌单的问题

# 参考
https://tianma.space/post/3998746934/index.html
https://clearsky.me/hexo-aplayer.html
<br>
<br>