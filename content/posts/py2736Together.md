---
title: Windows下Python2&Python3共存的解决方案
date: 2018-02-5T14:19:00+08:00
comments: true
toc: true
tags:
 - Python
---

近来做院创和新苗要用到OpenCV了，自己电脑装的是Python3.6，但是cv2得在Python2.7下跑，又懒得搞虚拟机，就搞了个双版本

<!-- more -->

首先，我电脑原来就有Python3.6并且已经添加到环境变量了，下面我们就从Python2.7安装开始说。

到官网下载安装[Python2.7.14](https://www.python.org/downloads/release/python-2714/)

注意：`不要添加到环境变量`

安装完后，打开2.7的安装位置。将`python.exe`、`pythonw.exe`修改为`python2.exe`、`pythonw2.exe`，如图

（这里因为我自己平时用3.6用的比较多，所以我选择只改python2.7的文件名）

<img src="http://p1u1vh70v.bkt.clouddn.com/py2.7&3.6%201.png" height="20%" width="20%">

然后打开`系统-高级系统设置-环境变量-Path-编辑`，如图

<img src="http://p1u1vh70v.bkt.clouddn.com/py2.7&3.6%202.png" height="80%" width="80%">

将Python2.7的路径添加进去即可，如图

<img src="http://p1u1vh70v.bkt.clouddn.com/py2.7&3.6%203.png" height="60%" width="60%">

然后在powershell里输入`python`即可进入python3，输入`python2`即可进入python2，如图

<img src="http://p1u1vh70v.bkt.clouddn.com/py2.7&3.6%204.png" height="80%" width="80%">

关于pip的使用也很简单。

在python2下装库的，输入`python2 -m pip install xxx`
在python3下装库的，输入`python -m pip install xxx`
其余pip操作同理，如图

<img src="http://p1u1vh70v.bkt.clouddn.com/py2.7&3.6%205.png" height="80%" width="80%">

<img src="http://p1u1vh70v.bkt.clouddn.com/py2.7&3.6%206.png" height="80%" width="80%">

然后，你就可以双版本上路，起飞了www