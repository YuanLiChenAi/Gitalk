---
title: markdown语法与hexo标签插件
date: 2018-09-29 15:06:20
updated: 2019-03-04 23:06:20
permalink: markdown&hexo
src: //dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/markdown%26hexo/title.jpg
categories: Code
tags:
    - hexo
    - markdown
---
{% blockquote %}
整理一下markdown语法和hexo标签插件的笔记
{% endblockquote %}
<!--more-->
# 区别
- ***Markdown是什么？***
>- Markdown是一种轻量级标记语言，它以纯文本形式(易读、易写、易更改)编写文档，并最终以HTML格式发布。
>- Markdown也可以理解为将以MARKDOWN语法编写的语言转换成HTML内容的工具。

- ___什么是 Hexo？___
>- Hexo 是一个快速、简洁且高效的博客框架。
>- Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
>- hexo标签插件可以实现部分markdown语法


# 引用
在文章中插入引言，可包含作者、来源和标题。

## markdown
### 语法
```
> 区块引用
>> 嵌套引用
>>> ...
```
### 示例
> 区块引用
>> 嵌套引用
>>>...    

### 注意点：
> 引用一个段落时可以在首行加`>`，空行自动结束

## hexo
### 语法
```
{% blockquote [author[, source]] [link] [source_link_title] %}
内容
{% endblockquote %}
```
### 示例
#### 没有提供参数，则只输出普通的 blockquote
```
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}
```
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}
#### 引用书上的句子
```
{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}
```
{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}
#### 引用 Twitter
```
{% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
NEW: DevDocs now comes with syntax highlighting. http://devdocs.io
{% endblockquote %}
```
{% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
NEW: DevDocs now comes with syntax highlighting. http://devdocs.io
{% endblockquote %}
#### 引用网络上的文章
```
{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}
```
{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}

## 小结
>- 若只要简单引用，markdown更方便。
>- 复杂引用更适合使用hexo插件
# 代码块
## markdown
### 语法
> &middot;&middot;&middot;[language] [title] [url] [link text] 
code snippet
&middot;&middot;&middot;    

### 示例
> &middot;&middot;&middot;py title https://baidu.com link-text 
import requests,re
r = requests.post('http://120.24.86.145:9001/test/',data={'clicks':1000000})
a = re.findall(r'flag{(.*)}', r.text)
print('flag{' + %a[0] + '}')
&middot;&middot;&middot;

```py title https://baidu.com link-text
import requests,re
r = requests.post('http://120.24.86.145:9001/test/',data={'clicks':1000000})
a = re.findall(r'flag{(.*)}', r.text)
print('flag{' + %a[0] + '}')
```

## hexo
### 语法
```
{% codeblock [title] [lang:language] [url] [link text] %}
code snippet
{% endcodeblock %}
```

### 示例
```
{% codeblock title lang:py https://baidu.com link text %}
import requests,re
r = requests.post('http://120.24.86.145:9001/test/',data={'clicks':1000000})
a = re.findall(r'flag{(.*)}', r.text)
print('flag{' + %a[0] + '}')
{% endcodeblock %}
```
{% codeblock title lang:py https://baidu.com link text %}
import requests,re
r = requests.post('http://120.24.86.145:9001/test/',data={'clicks':1000000})
a = re.findall(r'flag{(.*)}', r.text)
print('flag{' + %a[0] + '}')
{% endcodeblock %}

## 小结
> 两者功能相同，就简单而言，我选择markdown语法
顺带提一下今天写博客发现的bug下
```
print('flag{%s}'%a[0]) //这是上面代码原来的部分代码，如果没有用反引号引用代码的方式将他包在里面，会使得hexo崩溃
{% //排除之后是这玩意会使得hexo崩溃
{# //测试发现这样也行
```
![崩溃信息](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/markdown%26hexo/2018-09-29-16-56-38.png)

# hexo插件其他功能
## Pull Quote
在文章中插入 Pull quote。
```
{% pullquote [class] %}
content
{% endpullquote %}
```
{% pullquote [class] %}
content
{% endpullquote %}

## jsFiddle
在文章中嵌入 jsFiddle。
```
{% jsfiddle shorttag [tabs] [skin] [width] [height] %}
```

## Gist
在文章中嵌入 Gist。
```
{% gist gist_id [filename] %}
```

## iframe
在文章中插入 iframe。
```
{% iframe url [width] [height] %}
```

## Image
在文章中插入指定大小的图片。
```
{% img [class names] /path/to/image [width] [height] [title text [alt text]] %}
```

## Link
在文章中插入链接，并自动给外部链接添加 target="_blank" 属性。
```
{% link text url [external] [title] %}
```

## Include Code
插入 source 文件夹内的代码文件。
```
{% include_code [title] [lang:language] path/to/file %}
```

## Youtube
在文章中插入 Youtube 视频。
```
{% youtube video_id %}
```

## Vimeo
在文章中插入 Vimeo 视频。
```
{% vimeo video_id %}
```

## 引用文章
引用其他文章的链接。
```
{% post_path slug %}
{% post_link slug [title] %}
```

## 引用资源
引用文章的资源。
```
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```

## Raw
如果您想在文章中插入 Swig 标签，可以尝试使用 Raw 标签，以免发生解析异常。
```
{% raw %}
content
{% endraw %}
```

# markdown其他语法
## 列表
### 无序列表
```
- 1
- 2
```
- 1
- 2

### 有序列表
```
1. 一
2. 二
```
1. 一
2. 二

### 代办列表
表示列表是否勾选状态（注意：[ ] 前后都要有空格）
```
- [ ] 不勾选
- [x] 勾选
```
- [ ] 不勾选
- [x] 勾选

## ~~删除线~~
```
~~删除线~~
```

## 强调
### 斜体
```
*YuanLiChenAi*
_YuanLiChenAi_
```
*YuanLiChenAi*
_YuanLiChenAi_

### 加粗
```
**YuanLiChenAi**
__YuanLiChenAi__
```
**YuanLiChenAi**
__YuanLiChenAi__

### 加粗斜体
```
***YuanLiChenAi***
___YuanLiChenAi___
```
***YuanLiChenAi***
___YuanLiChenAi___

## 表格
```
First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell
```

First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell

或者也可以让表格两边内容对齐，中间内容居中，例如：
```
First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right
```

First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right

## 上标&下标
```
a<sup>2</sup>
H<sub>2</sub>O  CO<sub>2</sub>
# 使用数学公式
$2^2$
$H_2$
```
a<sup>2</sup>
H<sub>2</sub>O  CO<sub>2</sub>
$2^2$
$H_2$


<br>
<br>