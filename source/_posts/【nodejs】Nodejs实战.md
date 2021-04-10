---
title: Nodejs实战
description: '-'
tags:
  - WEB开发
  - Web后端
  - NodeJs
abbrlink: 47184e2d
date: 2021-01-20 00:15:23
---



# Node和浏览器工作原理
事件驱动（事件轮询）和非阻塞的I/O处理（异步IO）

Apache是采用带有阻塞的I/O的多线程HTTP服务器，NGINX和Node一样采用的是异步I/O的事件轮询

Node的全局对象是global，浏览器的全局对象是window。node重新实现了浏览器中的常见全局对象，是浏览器和服务器尽量保持一致。
如setTimeout()方法，console.log()方法

# DIRT程序
实际上，Node所针对的应用程序有一个专门的简称：DIRT。它表示==数据密集==型==实时==(data-intensive real-time)程序。

browserling.com就是一个用Node开发的DIRT程序，可以模拟运行浏览器所需的CPU和外设。


