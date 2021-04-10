---
title: linux添加环境变量的4种方法
description: '-'
tags:
  - 运维
  - Linux
abbrlink: 9d24c54e
date: 2021-04-01 23:31:40
---



**一、需要明白以下2点：**

1、Linux的环境变量是保存在变量PATH中，可通过Linux shell命令 `echo $PATH` 查看输出内容，或者直接输入`export`查看。

2、Linux环境变量值之间是通过冒号进行隔开的( : )

　　格式为：`PATH=$PATH:<PATH 1>:<PATH 2>:<PATH 3>:------:<PATH N>`

**二、暂时的添加环境变量PATH：**

可通过export命令，如

`export PATH=/usr/local/nginx/sbin/:$PATH`，将`/usr/local/nginx/sbin/`目录临时添加到环境变量中

**三、为当前用户永久添加环境变量：**

编辑 `.bashrc`  文件 `vim ~/.bashrc`  

文件末尾添加：`export PATH="/usr/local/nginx/sbin/:$PATH"`

然后再执行 `source ~/.bashrc`

**四、为所有用户永久添加某一环境变量：**

编辑  /etc/profile  文件 `vim /etc/profile`  

文件末尾添加：`export PATH="/usr/local/nginx/sbin/:$PATH"`

然后再执行 `source /etc/profile`

**五、/etc/environment 下面添加（网上查到貌似很容易被搞崩电脑，就不放这个了）**



**推荐阅读：**

[linux不同环境变量文件的比较，如/etc/profile和/etc/environment](https://blog.csdn.net/qq_34823218/article/details/106581446)