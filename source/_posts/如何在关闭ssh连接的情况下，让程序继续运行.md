---
title: 如何在关闭ssh连接的情况下，让程序继续运行
description: 暂无描述！
tags:
  - 运维
  - Linux
abbrlink: ef747403
date: 2021-01-20 00:15:19
---



## 前言

系统版本：`Ubuntu16.04`

软件：`screen`， `mobaXterm`

我们通过SSH去连接linux服务器时，当我们关闭SSH连接的话，当前正在执行的脚本文件也会被停止，因为linux服务器会在你退出SSH连接后，默认关闭进程，所以不想一直保持SSH连接，又想一直保持服务器程序运行该怎么办呢？



## 正文

### 安装screen

```bash
sudo apt-get install screen 
```

### 创建screen会话

```bash
screen -S test
```

> test 是screen虚拟终端的名称，可自定义

### 运行程序

运行一个需要在后台一直运行的程序，如我这里举例就是开启一个静态服务器端口

```bash
python -m SimpleHTTPServer
```

![1610344448968](assets/如何在关闭ssh连接的情况下，让程序继续运行/1610344448968.png)

### 关闭终端

这时候服务还是在后台运行。

![1610344534481](assets/如何在关闭ssh连接的情况下，让程序继续运行/1610344534481.png)

### 查看screen会话

```bash
screen -ls
```

![1610344635039](assets/如何在关闭ssh连接的情况下，让程序继续运行/1610344635039.png)

### 进入会话

```bash
screen -r 63017
```

> 63017 是上面查看的会话id， 输入后就可以看到之前运行的命令了。

![1610344708963](assets/如何在关闭ssh连接的情况下，让程序继续运行/1610344708963.png)

### 退出会话

```bash
exit
```

![1610344770289](assets/如何在关闭ssh连接的情况下，让程序继续运行/1610344770289.png)





> screen用法：
>
> - 打开新的会话窗口：screen
> - 结束当前会话：exit
> - 在新会话中执行程序（程序关闭时会话自动结束）：screen vi test.c
> - 打开新会话并起个名字：screen -S myname
> - 暂时离开会话（经常用）：Ctrl+a 然后 d
> - 查看会话列表: screen -ls
> - 恢复之前离开的会话：screen -r 会话名或进程号
> - 清除dead状态的会话：screen -wipe
> - 启动一个开始就是Detached状态的会话：screen -dmS 名字 命令

