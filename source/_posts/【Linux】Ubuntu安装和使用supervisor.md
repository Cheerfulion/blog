---
title: 【Linux】Ubuntu安装和使用supervisor
description: '-'
tags:
  - 运维
  - Linux
abbrlink: 2381c731
date: 2021-03-31 21:44:51
---



## 前言
对于需要以进程的方式常驻在Ubuntu系统中或开机启动的脚本程序，通常使用supervisor进程管理工具进行管理。本文将简单介绍supervisor进程管理工具的安装和使用。



## 安装

```
sudo apt-get install supervisor
```



## 设置开机启动

```bash
 systemctl enable supervisor
 # 重启服务
 systemctl restart supervisor  
```



## 开启允许浏览器访问

```bash
# 修改配置文件
vim /etc/supervisor/supervisord.conf

# 增加内容如下
[inet_http_server]
port=0.0.0.0:9001
username =admin
password =admin

# 重启服务
systemctl restart supervisor  
```



## 新建进程配置

安装supervsor进程管理工具后，建议在`/etc/supervisor/conf.d/`文件夹中为每一个进程创建一个进程配置。

```bash
cd /etc/supervisor/conf.d/
sudo vim web.conf
```



## 配置详解

```bash
[program:web]
command=sh /usr/local/bin/test.sh   ; 被监控的进程路径
priority=999						; 指明进程启动和关闭的顺序
numprocs=1                    		; 启动一个进程
directory=/usr/local/bin/     		; 执行前切换路径
autostart=true                		; 是否随supervisord启动而启动
autorestart=true              		; 进程意外退出后是否自动重启
startretries=10                 	; 启动失败时的最多重试次数
exitcodes=0                     	; 正常退出代码
stopsignal=KILL               		; 用来杀死进程的信号
stopwaitsecs=10               		; 发送SIGKILL前的等待时间
redirect_stderr=true          		; 重定向stderr到stdout
stdout_logfile=logfile        		; 指定日志文件
```

**我的配置**

```bash
[program:web]
directory= /var/www/www_ionluo_cn
command = npm run server
# user = www-data
autostart = true
autorestart = true
stopsignal = QUIT
# redirect_stderr = true
# loglevel = info
# stdout_logfile = /var/log/supervisor/web.log
# stderr_logfile = /var/log/supervisor/web_err.log
# logfile_maxbytes = 1M
```



## 启动进程

启动进程

```
supervisorctl reload
supervisorctl start test
```



## 命令详解

```bash
#启动进程
supervisorctl start xxx
#重启进程
supervisorctl restart xxx
#重启所有属于名为group的分组进程
supervisorctl stop group
#停止全部进程
supervisorctl stop all
#载入最新配置的文件
supervisorctl reload
#根据最新的配置文件，启动新配置或有改动的进程
supervisorctl update
```



## 问题记录

我这里遇到python不存在和true标识符不对等问题，其实都是配置文件问题，可以用我的配置文件尝试。



## 参考

https://blog.csdn.net/qq_32013641/article/details/88637280