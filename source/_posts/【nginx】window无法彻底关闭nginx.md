---
title: window无法彻底关闭nginx
description: '-'
tags:
  - Nginx
abbrlink: e8f75a46
date: 2021-03-31 20:15:42
---



window中任务管理器和命令`nginx -s stop`发现都无法关闭`nginx`。



可用下面命令关闭：

```powershell
taskkill /f /t /im nginx.exe
```



![image-20210331142550393](http://blog.cdn.ionluo.cn/blog/image-20210331142550393.png)