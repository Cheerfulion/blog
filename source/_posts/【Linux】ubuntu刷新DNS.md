---
title: ubuntu刷新DNS
description: '-'
tags:
  - 运维
  - Linux
abbrlink: ca173be1
date: 2021-02-23 21:51:33
---



[Linux](http://lib.csdn.net/base/linux)刷新dns的缓存方法是：

```bash
sudo /etc/init.d/nscd restart
```

如果发现提示命令找不到：

```bash
sudo: /etc/init.d/nscd: command not found
```

后来发现是需要先安装nscd包：

```bash
sudo apt-get install nscd
```

**最暴力的方法刷dns，重启网络：**

```bash
sudo /etc/init.d/networking restart
```



Windows刷新dns的缓存方法是：

```bash
ipconfig /flushdns
```

