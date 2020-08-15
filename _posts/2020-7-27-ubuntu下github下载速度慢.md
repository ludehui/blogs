---
published: true
title: ubuntu下github克隆速度慢解决方案
category: 
  - git
  - ubuntu
  - Solution
tags: 
  - Solution
  - linux
layout: post
---



# ubuntu下github克隆速度慢解决方案

## 方案一

### 1. 追加IP域名IP地址

我们可以打开[IPAddress](https://www.ipaddress.com/ )，如下图所示：

![avator](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMuY25ibG9ncy5jb20vY25ibG9nc19jb20vbHV0YWlzaGkvMTgxNTAyNS9vXzIwMDcyNzEzMDUxNUlQQWRkcmVzcy5wbmc?x-oss-process=image/format,png)

来获得以下两个GitHub域名的IP地址：

(1) github.com

(2) github.global.ssl.fastly.net

以github.com为例，获得IP Address为140.82.112.4。

![avator](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMuY25ibG9ncy5jb20vY25ibG9nc19jb20vbHV0YWlzaGkvMTgxNTAyNS9vXzIwMDcyNzEzMDUyMGdpdGh1Yi5jb20ucG5n?x-oss-process=image/format,png)

同理，可以得到github.global.ssl.fastly.net的IP Address。

### 2. 修改hosts文件

ubuntu下hosts路径为：

```
/etc/hosts
```

使用vim打开

```
$ sudo vim hosts
```
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMuY25ibG9ncy5jb20vY25ibG9nc19jb20vbHV0YWlzaGkvMTgxNTAyNS9vXzIwMDcyNzEzMDUwOGhvc3RzLnBuZw?x-oss-process=image/format,png)

在文件后边追加IP Address和域名。

## 方案二

使用github加速插件，点击[这里]([https://chrome.google.com/webstore/detail/github%E5%8A%A0%E9%80%9F/kejahdakjmkfddgnifodfnpcklckjjpo/related](https://chrome.google.com/webstore/detail/github加速/kejahdakjmkfddgnifodfnpcklckjjpo/related))下载安装。
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMuY25ibG9ncy5jb20vY25ibG9nc19jb20vbHV0YWlzaGkvMTgxNTAyNS9vXzIwMDgwNDEzMzA0OWdpdGh1YmFjYy5wbmc?x-oss-process=image/format,png)

安装之后，再打开github界面，会多出一个按钮，如下图所示：![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMuY25ibG9ncy5jb20vY25ibG9nc19jb20vbHV0YWlzaGkvMTgxNTAyNS9vXzIwMDgwNDEzMzMwOGNsb25lYWNjLnBuZw?x-oss-process=image/format,png)
然后复制链接，使用git clone 指令克隆，网速非常快。![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMuY25ibG9ncy5jb20vY25ibG9nc19jb20vbHV0YWlzaGkvMTgxNTAyNS9vXzIwMDgwNDEzMzQwOWFjYy5wbmc?x-oss-process=image/format,png)]