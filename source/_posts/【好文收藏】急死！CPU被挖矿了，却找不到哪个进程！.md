---
title: 【好文收藏】急死！CPU被挖矿了，却找不到哪个进程！
description: 暂无描述！
tags:
  - 运维
  - Linux
abbrlink: f31b5257
date: 2021-03-12 23:00:31
---



原文链接：https://mp.weixin.qq.com/s/5xvxjORUL6Rwe3hhcsryfA



总结：

对于挖矿程序来说，主要占用的资源是CPU和GPU，也会消耗一定的内存。对于一些普通服务器，一般都是占用CPU，因此找出的方法主要就**查看CPU占用高的进程**。

1. 可以使用`top`或者`htop`（需要安装）来查看，如下图第一个就是一个挖矿程序

   ![image-20210310092321671](http://blog.cdn.ionluo.cn/blog/image-20210310092321671.png)

2. 如果找不到，可以使用`unhide`命令来查找隐藏进程

   > unhide需要安装，ubuntu安装命令：`sudo apt-get install unhide`
   >
   > 查找：`sudo unhide proc`

3. 如果上面两种方法都无法定位，可以参考文章的端口排查法。



**找到程序后，需要分析该进程的信息，什么时候开启的，如何开启的等等。**

Linux在启动一个进程时，系统会在`/proc`下创建一个以PID命名的文件夹，在该文件夹下会有我们的进程的信息，其中包括一个名为exe的文件即记录了绝对路径，通过`ll`或`ls –l`命令即可查看。

```bash
# 10822是上图中的进程PID
ll /proc/10822
```

