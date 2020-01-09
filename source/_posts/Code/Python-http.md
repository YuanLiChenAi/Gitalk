---
title: Python-http
date: 2018-04-15 20:32:09
updated: 2018-04-15 20:32:09
permalink: Python-http
categories: Code
tags:
    - Python
    - HTTP
---
{% blockquote %}
Python爬虫使用urllib and requests
{% endblockquote %}
<!--more-->

# urllib
参考资料：
[python爬虫从入门到放弃（三）之 Urllib库的基本使用](http://www.cnblogs.com/zhaof/p/6910871.html)
[Python爬虫入门三之Urllib库的基本使用](https://cuiqingcai.com/947.html)
[Python3的urllib.parse常用函数小结](http://www.jb51.net/article/92818.htm)
[python3 get和post请求](https://www.jianshu.com/p/9e50c58dabdd)

## 模块
{% blockquote %}
urllib.request 请求模块
urllib.error 异常处理模块
urllib.parse url解析模块
urllib.robotparser robots.txt解析模块
{% endblockquote %}
```python
from urllib import request # 导入模块
```

## request.Request
```python
req = request.Request("url") # 创建一个url对象
req.add_header("Cookie", "PHPSESSID=ga0un6plm7tea9li11bgnommh1") # 添加头部信息,例如cookie,多个信息时用分号隔开
```
两种添加头部信息的方法
```python
from urllib import request, parse

url = 'http://httpbin.org/post'
headers = {
    'User-Agent': 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)',
    'Host': 'httpbin.org'
}
dict = {
    'name': 'zhaofan'
}
data = bytes(parse.urlencode(dict), encoding='utf8')
req = request.Request(url=url, data=data, headers=headers, method='POST')
response = request.urlopen(req)
print(response.read().decode('utf-8'))
```
```python
from urllib import request, parse

url = 'http://httpbin.org/post'
dict = {
    'name': 'Germey'
}
data = bytes(parse.urlencode(dict), encoding='utf8')
req = request.Request(url=url, data=data, method='POST')
req.add_header("Cookie", "PHPSESSID=ga0un6plm7tea9li11bgnommh1")
response = request.urlopen(req)
print(response.read().decode('utf-8'))
```

## request.urlopen
```python
urlopen(url, data, timeout)
# 第一个参数url即为URL，第二个参数data是访问URL时要传送的数据，第三个timeout是设置超时时间。
# 第二三个参数是可以不传送的，data默认为空None，timeout默认为 socket._GLOBAL_DEFAULT_TIMEOUT。
# 第一个参数URL是必须要传送的，在这个例子里面我们传送了百度的URL，执行urlopen方法之后，返回一个response对象，返回信息便保存在这里面。
```

### 打开一个百度链接
```python
from urllib import request

response = request.urlopen("http://www.baidu.com")
print(response.read().decode('utf-8')) # 查看返回信息必须带上read(),否则返回对象描述，decode()解码大多数为‘utf-8’或‘gbk’
print(response.getheaders()) # 查看响应头
```
可以用for循环修改打印的格式
```python
for a,b in response.getheaders():
    print('%s: %s' % (a, b))
```
```http
Date: Mon, 16 Apr 2018 02:31:13 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: Close
Vary: Accept-Encoding
Set-Cookie: BAIDUID=05730C3C016AA6027A951688792E8891:FG=1; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
Set-Cookie: BIDUPSID=05730C3C016AA6027A951688792E8891; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
Set-Cookie: PSTM=1523845873; expires=Thu, 31-Dec-37 23:55:55 GMT; max-age=2147483647; path=/; domain=.baidu.com
Set-Cookie: BDSVRTM=0; path=/
Set-Cookie: BD_HOME=0; path=/
Set-Cookie: H_PS_PSSID=1456_21112_20929; path=/; domain=.baidu.com
P3P: CP=" OTI DSP COR IVA OUR IND COM "
Cache-Control: private
Cxy_all: baidu+6d6c985d736e75076f923a5ec17088e3
Expires: Mon, 16 Apr 2018 02:30:23 GMT
X-Powered-By: HPHP
Server: BWS/1.1
X-UA-Compatible: IE=Edge,chrome=1
BDPAGETYPE: 1
BDQID: 0xee6d60d000001ac3
BDUSERID: 0
```

<br>
## urllib.parse url解析薄块
```python
from urllib import parse # 导入模块
```
不知道怎么讲，直接上一道CTFweb题目吧，这是实验吧的一道web题，
原题链接：[http://www.shiyanbar.com/ctf/1854](http://www.shiyanbar.com/ctf/1854)
解题链接：[http://ctf5.shiyanbar.com/web/10/10.php](http://ctf5.shiyanbar.com/web/10/10.php)
```python
from urllib import request,parse
import base64

def main():
    url = 'http://ctf5.shiyanbar.com/web/10/10.php'
    req = request.Request(url)
    req.add_header("Cookie", "PHPSESSID=ga0un6plm7tea9li11bgnommh1") # 添加cookie
    response = request.urlopen(req) # 打开链接
    head = response.getheaders()[3][1] # 获取头部信息
    post = base64.b64decode(head).decode('utf-8')[25:] # base64解密
    data = parse.urlencode([('key',post)])  # 将值加入post数据中
    response = request.urlopen(req,data.encode('utf-8')) # 打开链接并添加utf-8编码后的数据
    print(response.read().decode('utf-8')) # 读取结果并utf-8解码


main()
```

## get请求
```python
from urllib import request,parse

data={}
data['name']='aaa'
url_parame=parse.urlencode(data)
url="http://xxxx"
all_url=url+url_parame
response=request.urlopen(all_url).read()
print(response.read())
```

## post请求
```python
from urllib import request,parse

url = 'http://xxxxxx'
user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)'
values = {'name' : 'aaa'}
headers = { 'User-Agent' : user_agent }
data = parse.urlencode(values)
req = request.Request(url+'?'+data)
response = request.urlopen(req)
print(response.read().decode('utf-8'))
```

<br>
<br>
# requests
参考资料：
[http://docs.python-requests.org/zh_CN/latest/user/quickstart.html](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html)
[http://docs.python-requests.org/zh_CN/latest/user/advanced.html](http://docs.python-requests.org/zh_CN/latest/user/advanced.html)

## 安装requests
{% blockquote %}
pip install requests
pip list # 查看
{% endblockquote %}

## get请求
```python
import requests

r = requests.get("http://xxxxx?name=aaa")
print(r.text)
```

## post请求
```python
import requests

postdata = { 'name':'aaa' }
r = requests.post("http://xxxxx?name=aaa",data=postdata)
print(r.text)
```

<br>
<br>
