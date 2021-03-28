---
title: ubuntu用户管理
description: 暂无描述！
tags:
  - 运维
  - Linux
abbrlink: '98e01648'
date: 2021-02-16 08:43:30
---



## 1. 创建用户

在命令行中执行以下操作：

`sudo useradd [username] -m`

注意要在后面加`-m`，否则不会在home路径下创建该用户的文件夹

创建好之后可以在`/home/`路径下查看该用户名的文件夹

在CLI中执行`cat /etc/passwd`可以查看passwd文件中是否有刚才添加的用户名，如果有，则表示添加成功

## 2. 设置密码

`sudo passwd [username]`

在弹出来的提示窗口中设置密码即可

## 3. 切换用户

`su [username]`

输入密码后即可切换到该用户

## 4. 删除用户

`sudo userdel -r [username]`

加上`-r`可以删除`/home/`路径下的用户文件夹，否则不能

## 5. 给用户绑定bash

`sudo usermod -s /bin/bash [username]`

不绑定bash的话，远程登录切换到新建用户的终端，只是显示$符号。

