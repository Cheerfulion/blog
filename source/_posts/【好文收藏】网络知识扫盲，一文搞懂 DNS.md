---
title: 【好文收藏】网络知识扫盲，一文搞懂 DNS
description: '-'
tags:
  - 运维
  - Linux
abbrlink: 4bf291f5
date: 2021-03-31 20:15:42
---



>  原文地址：https://mp.weixin.qq.com/s/7jyJLz-Hyvr_-qc22m5i-g

## **手动清理本地缓存**

MacOS

```
$ sudo dscacheutil -flushcache$ sudo killall -HUP mDNSResponder
```

Windows

```
$ ipconfig /flushdns
```

Linux

```
# 使用NSCD的DNS缓存
$ sudo /etc/init.d/nscd restart
# 服务器或者路由器使用DNSMASQ
$ sudo dnsmasq restart
```