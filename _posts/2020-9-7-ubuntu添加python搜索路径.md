---
published: true
title: 为Python添加默认模块搜索路径
category: python
tags:
   - python
   - 搜索路径
layout: post
---



## **为Python添加默认模块搜索路径**

#### 方法一:函数添加

1 import sys
2 查看sys.path
3 添加sys.path.append("c:\\")

#### 方法二:修改环境变量

w用户可以修改系统环境变量PYTHONPATH

#### 方法三:增加.pth文件

在site-packages添加一个路径文件，如mypkpath.pth，必须以.pth为后缀，写上你要加入的模块文件所在的目录名称就是了。
 1 windows
  c:\python27\site-packages
 2 linux(ubuntu)
  /data/.env/pytorch/ython3.6/lib/site-packages
2 linux(redhat)
  /usr/lib/python2.7/site-packages