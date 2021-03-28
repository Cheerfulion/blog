---
title: 谷歌浏览器（chrome）允许跨域设置的方法
description: 暂无描述！
tags:
  - 黑科技
abbrlink: b1f47f66
date: 2021-02-12 13:31:15
---



### 什么是跨域？

跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器施加的安全限制。

简单的说，跨域是浏览器的限制。

### 允许跨域有什么用呢？

允许跨域则可以访问其他的内容。均益在做网站前后端分离开发的时候，经常遇到跨域的问题。通过在开发过程中，解决跨域的问题有三种：

1. jsonp方式
2. 代理服务器的方式
3. 服务端允许跨域访问
4. 取消浏览器的跨域限制

这里均益主要讲取消谷歌浏览器的跨域限制，因为这种方式在开发阶段最简单。

### 命令行的方式
#### Windows

直接创建chrome浏览器的快捷方式，在属性中找到打开路径，在…chrome.exe后面加上

```bash
--args --disable-web-security --user-data-dir="C:/ChromeDevSession"
```

#### Mac

在终端中执行命令

```bash
open -a 'Google Chrome' --args --disable-web-security --user-data-dir=/tmp/chrome_dev_test
```

执行成功，会看到浏览器顶部有一个提示，说明取消跨域成功

```
您使用的是不受支持的命令行标志：--disable-web-security ，稳定性和安全性会有所下降。
```



> 上诉方式是对于新版chrome浏览器设置的设置，普通的也可以通过参数`--disable-web-security`直接设置。如果上诉方式之后依旧没有出现提示，可以参考这个文章解决：https://www.jb51.net/css/754310.html

### 扩展程序的方式

谷歌浏览器有很多好用的扩展程序，但是由于墙的原因，访问不到。这个取消跨域限制的插件是

[Allow-Control-Allow-Origin](https://chrome.google.com/webstore/detail/allow-cors-access-control/lhobafahddgcelffkeicbaginigeejlf?hl=zh-CN) 需要科学上网





> 转自：https://junyiseo.com/qita/792.html