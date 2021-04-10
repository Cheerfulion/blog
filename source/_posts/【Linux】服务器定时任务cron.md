---
title: 【cron】服务器定时任务
tags:
  - 运维
  - Linux
abbrlink: 8586c35a
date: 2021-04-08 11:39:35
---



## 前言

我的项目经常需要更新，但是搭建自动部署系统稍微麻烦一些，所以打算用cron来做个定时脚本更新项目。

> PS: 如果项目部署可能存在失败的情况一定要搭建下自动部署的系统，因为脚本的方式失败会直接影响到线上环境。



## 介绍

cron是一个Linux下的后台进程，用来定期的执行一些任务。



**表达式生成工具和教程见：**

[www.pppet.net](https://www.pppet.net/)

>  系统中没有秒这个单位，去掉秒即是系统的cron表达式。



## 使用

1. **查看cron是否运行**

   ```bash
   ps -ef | grep cron
   ```

2. **启动cron**

   ```bash
   sudo service cron start
   ```

3. **编辑cron脚本**

   ```bash
   # 第一次使用会让你选择默认编辑器，我选用了vim
   sudo crontab -e
   
   # 加入脚本， 该脚本的作用是每30分钟到项目的根目录（/var/www/www_ionluo_cn/blog）拉取下代码，并把日志写入（/var/log/cron/www_ionluo_cn.log）
   */30 * * * * cd /var/www/www_ionluo_cn/blog && git pull >> /var/log/cron/www_ionluo_cn.log 2>&1
   
   # 如果一直看不到日志文件，可以先提前创建好该文件！！！
   ```

4. **重启cron服务以应用脚本**

   ```bash
   sudo service cron restart
   ```

5. **OK，大功告成！接下来就可以在日志文件中查看日志来验证脚本的执行情况了。**



我比较推荐给每个任务单独的日志存储，但是如果你不希望这么做，而是保存所有任务到一个日志文件，或者需要邮件告警，可以开启cron日志记录功能。



## 开启cron日志记录

1. **修改rsyslog**

   ```bash
   vim /etc/rsyslog.d/50-default.conf 
   
   # 去掉下面这行的注释
   cron.* /var/log/cron.log 
   ```

2. **重启rsyslog**

   ```bash
   service rsyslog restart
   ```

3. **查看crontab日志**

   ```bash
   tail /var/log/cron.log
   ```



> 注意：当配置了服务器的邮件发送功能后，crontab的定时任务执行后都会给用户发送一封邮件，通过以下方法可以防止这种情况。
>
> 参考：[设置Crontab执行任务时不发送邮件](https://blog.csdn.net/E_Possible/article/details/108065129)
>
> ```bash
> #这是第一种方法，设置MAILTO参数为空
> MAILTO=""
> 
> #这是第二种方法，将命令定向到/dev/null 2>&1
> 0-59/30 * * * * root /opt/freemem/freemen >/dev/null 2>&1
> 
> #这是第三种方法，将命令定向到/dev/null
> 30 3 * * * root /backup/sh/mysql_data_bk.sh &>/dev/null
> ```









