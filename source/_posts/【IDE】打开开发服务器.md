---
title: 打开开发服务器
description: '-'
tags:
  - IDE
  - VSCode
abbrlink: '78059394'
date: 2021-01-04 22:17:45
---



有的时候，写小demo的单纯html文件的时候，发现VSCode不能使用浏览器打开的选项，这里罗列下我用的一些创建简单http服务器的方式。



## 一、http-server

```bash
# 安装
npm install http-server -g
# 在当前目录开启http服务
http-server
```



## 二、插件Live Server

vscode插件库安装，安装好后直接右击 `Open with Live Server` 打开即可！



## 三、其他语言的服务器

比较简单并且Linux下默认安装的python是首选选择

**Python 2.x**

```python
python -m SimpleHTTPServer
```

**Python 3.x**

```python
python -m http.server
```

>  扩展：[Python的-m参数](https://www.cnblogs.com/maoguy/p/6670988.html)