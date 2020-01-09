---
title: Pycharm使用遇到的问题
date: 2018-05-21 10:33:37
updated: 2018-05-21 10:33:37
permalink: Pycharm
src: //dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Pycharm/title.jpg
categories: Code
tags:
    - Python
    - Pycharm
---
{% blockquote %}
本文用来积累pycharm使用过程中的各种问题
{% endblockquote %}
<!--more-->

# pip升级问题
问题描述：在venv虚拟环境中升级pip，升级失败
升级命令
```bash
python -m pip install --upgrade pip
```
失败报错
```bash
(venv) C:\Users\zxw\PycharmProjects\test>python -m pip install --upgrade pip
Collecting pip
  Using cached https://files.pythonhosted.org/packages/0f/74/ecd13431bcc456ed390b44c8a6e917c1820365cbebcb6a8974d1cd045ab4/pip-10.0.1-py2.py3-none-any.whl
Installing collected packages: pip
  Found existing installation: pip 9.0.3
    Uninstalling pip-9.0.3:
      Successfully uninstalled pip-9.0.3
  Rolling back uninstall of pip
Exception:
Traceback (most recent call last):
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\basecommand.py", line 215, in main
    status = self.run(options, args)
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\commands\install.py", line 342, in run
    prefix=options.prefix_path,
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\req\req_set.py", line 784, in install
    **kwargs
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\req\req_install.py", line 851, in install
    self.move_wheel_files(self.source_dir, root=root, prefix=prefix)
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\req\req_install.py", line 1064, in move_wheel_files
    isolated=self.isolated,
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\wheel.py", line 462, in move_wheel_files
    generated.extend(maker.make(spec))
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\_vendor\distlib\scripts.py", line 372, in make
    self._make_script(entry, filenames, options=options)
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\_vendor\distlib\scripts.py", line 276, in _make_script
    self._write_script(scriptnames, shebang, script, filenames, ext)
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\_vendor\distlib\scripts.py", line 212, in _write_script
    launcher = self._get_launcher('t')
  File "C:\Users\zxw\PycharmProjects\test\venv\lib\site-packages\pip-9.0.3-py3.6.egg\pip\_vendor\distlib\scripts.py", line 351, in _get_launcher
    result = finder(distlib_package).find(name).bytes
AttributeError: 'NoneType' object has no attribute 'bytes'
```
解决办法
```bash
python -m pip install -U --force-reinstall pip
```

# 安装pycrypto问题
## for python2.7
参考：[http://yunnigu.dropsec.xyz/2016/10/18/安装pycrypto遇到的一系列问题总结/](http://yunnigu.dropsec.xyz/2016/10/18/%E5%AE%89%E8%A3%85pycrypto%E9%81%87%E5%88%B0%E7%9A%84%E4%B8%80%E7%B3%BB%E5%88%97%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93/)
在安装pycrypto的时候出现unable to find vcvarsall.bat
问题原因：
很多python 库实际上使用c或者c++写的，所以安装编译时会需要用到msvc的东西；如果你的机器里没有装VS或者注册表设置不太对的话，就会报错。
微软出了一个msi包来解决这个问题：
Microsoft Visual C++ Compiler for Python 2.7
下载地址：http://www.microsoft.com/en-us/download/details.aspx?id=44266

# 安装时编译错误
## 错误描述
``` 
error: Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools": http://landinghub.visualstudio.com/visual-cpp-build-toolsfor
```
## 解决办法
### 安装VS studio
略。。
### 直接下载编译好的whl
下载地址：[Unofficial Windows Binaries for Python Extension Packages](https://www.lfd.uci.edu/~gohlke/pythonlibs/)
以gmpy2为例，`Ctrl + f`找到对应python版本的gmpy2的whl文件（此文件pip可解析）
![gmpy2](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Pycharm/2018-08-23-14-45-13.png)
下载后`pip install 文件名`安装即可
![Pycharm虚拟环境的可在控制台中安装](https://dev.tencent.com/u/YuanLiChenAi/p/BP/git/raw/master/blog/Pycharm/2018-08-23-14-49-37.png)

<br>
<br>
