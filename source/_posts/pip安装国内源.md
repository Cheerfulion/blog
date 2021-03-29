---
title: pip安装国内源
description: '-'
tags:
  - python
abbrlink: 7a2a044e
date: 2021-01-20 00:15:18
---



pip国内的一些镜像

阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣(douban)http://pypi.douban.com/simple/
清华大学https://pypi.tuna.tsinghua.edu.cn/simple/
中国科学技术大学http://pypi.mirrors.ustc.edu.cn/simple/

临时使用：
可以在使用pip的时候在后面加上-i参数，指定pip源
eg: pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple

