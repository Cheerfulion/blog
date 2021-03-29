---
title: wxhtmltopdf
description: '-'
tags:
  - WEB开发
  - Web后端
  - Django
abbrlink: 9d50cee6
date: 2021-03-29 23:11:49
---





## 文档

https://wkhtmltopdf.org/usage/wkhtmltopdf.txt



## `Exit with code 1 due to network error:  ContentNotFoundError`

![1609915985022](http://blog.cdn.ionluo.cn/blog/1609915985022.png)

原因是：此异常为是因为引用了外部的资源，如：字体，图片，iframe加载等。检查对应的外部资源是否可以加载。

## `returned non-zero exit status 1`

![1609921772044](http://blog.cdn.ionluo.cn/blog/1609921772044.png)

绝大多数报错都是返回这个的，需要在服务器上执行下这个命令拿到更详细的报错信息：

```bash
/var/www/beta_bio_cyagen_net/bin/wkhtmltopdf/0.12.3/wkhtmltopdf-amd64 --enable-local-file-access --encoding utf8 --footer-html /tmp/wkhtmltopdfNy5zuH.html --header-html /tmp/wkhtmltopdfGmy8DK.html --margin-bottom 10 --margin-top 16 --margin-bottom 10 --margin-top 15 --page-size A3 --quiet False /tmp/wkhtmltopdfRBSsIs.html -
```

![1609922196902](http://blog.cdn.ionluo.cn/blog/1609922196902.png)

可以看到，这里是权限拒绝，命令前面加上`sudo`, 输入root密码再执行一次。

![1609922338350](http://blog.cdn.ionluo.cn/blog/1609922338350.png)

这个问题看了GitHub，没有什么头绪。先看下是否可以打印网页，

```bash
sudo /var/www/beta_bio_cyagen_net/bin/wkhtmltopdf/0.12.3/wkhtmltopdf-amd64 'https://www.baidu.com' /tmp/baidu.pdf
```

依旧如此：

![1609923119400](http://blog.cdn.ionluo.cn/blog/1609923119400.png)

这很令人疑惑，查看下系统信息和软件版本：

```bash
cyagen@QhBetaBio:/$ lsb_release -a 
# No LSB modules are available.
# Distributor ID:	Ubuntu
# Description:	Ubuntu 18.04.1 LTS
# Release:	18.04
# Codename:	bionic

cyagen@QhBetaBio:/$ sudo /var/www/beta_bio_cyagen_net/bin/wkhtmltopdf/0.12.3/wkhtmltopdf-amd64 --version
# wkhtmltopdf 0.12.3 (with patched qt)
```

发现Ubuntu被运维升级成了18版本，之前也是18版本系统运行不了这个版本的软件。





## 封面无法去除页边距

https://github.com/wkhtmltopdf/wkhtmltopdf/issues/3579

https://github.com/wkhtmltopdf/wkhtmltopdf/issues/3579



## 封面参数需要移至全局参数之后

https://github.com/jgm/pandoc/issues/6171