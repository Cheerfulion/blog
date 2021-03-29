---
title: 未解决问题记录
description: '-'
tags:
  - 数据库
  - mysql
abbrlink: cf3c21ba
date: 2021-03-06 12:57:00
---



### 1. 修改mysql的缓存目录

因为公司的海外服务器经常丢包，会较为频繁更换公网IP，但是发现公网IP更换后 /tmp 目录的权限会重置（成drwx------T）。但是mysql的缓存目录需要权限777， 所以希望修改mysql的缓存目录。

> tmpdir = /tmp   =>  /tmpdir

重启mysql ( `service mysql restart` ), 报错：`Job for mysql.service failed because the control process exited with error code. See "systemctl status mysql.service" and "journalctl -xe" for details.`

日志显示：

![image-20210304114425960](http://blog.cdn.ionluo.cn/blog/image-20210304114425960.png)

文件夹权限：

![image-20210304114443579](http://blog.cdn.ionluo.cn/blog/image-20210304114443579.png)

**未解决**