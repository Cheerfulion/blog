---
title: 报错记录
description: '-'
tags:
  - Redis
abbrlink: 1718214a
date: 2021-03-06 12:57:00
---



## 1. not able to persist on disk
```bash
MISCONF Redis is configured to save RDB snapshots, but it is currently not able to persist on disk. Commands that may modify the data set are disabled, because this instance is configured to report errors during writes if RDB snapshotting fails (stop-writes-on-bgsave-error option). Please check the Redis logs for details about the RDB error.
```

原因：强制关闭Redis快照导致不能持久化。

解决方法：

```bash
# 1. 将stop-writes-on-bgsave-error设置为no （重启失效）
redis-cli
config set stop-writes-on-bgsave-error no
# 2. 将stop-writes-on-bgsave-error设置为no （永久生效）
vim /etc/redis/redis.conf
# 修改stop-writes-on-bgsave-error的值为no
# 重新启动您的Redis服务器
# MACOS（BREW）
brew services restart redis
# Linux
sudo service redis restart /sudo systemctl restart redis
# Windows
Windows + R->输入services.msc，找到Redis，然后单击restart
```



上面的解决方法是忽略错误，并不是解决错误，至于我们这里我是参考下面文章设置的：

https://blog.csdn.net/weixin_41866960/article/details/89608666

> 最近将一个项目部署到了腾讯云上，项目里用了redis做数据缓存。运行了三天都没有什么问题，但是到了第四天我进入网站某个页面时出现了bug，截图如下
>
> ![img](http://blog.cdn.ionluo.cn/blog/20190427221345687.png)
>
> `org.springframework.data.redis.RedisConnectionFailureException: Cannot get Jedis connection; nested exception is redis.clients.jedis.exceptions.JedisConnectionException: Could not get a resource from the pool`
>
> 异常里面显示无法连接上redis ，第一时间我去查看服务器上redis的运行情况，发现redis仍然是在运行中的，然后当我想ping一下redis测试一下的时候，出现了以下错误
>
> `MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk`
>
>  意思是：Redis被配置为保存数据库快照，但它目前不能持久化到硬盘。用来修改集合数据的命令不能用。请查看Redis日志的详细错误信息。
>
> 去网上搜了一下这个问题，网上说原因是强制关闭Redis快照导致不能持久化。给出的解决方案是运行`config set stop-writes-on-bgsave-error no`　命令后，关闭配置项`stop-writes-on-bgsave-error`解决该问题。
>
> ```bash
> root@ubuntu:# redis-cli
> 127.0.0.1:6379> config set stop-writes-on-bgsave-error no
> ```
>
> 完成以上操作后，重新刷新网页~真的可以了，牛逼！！然后我就去睡觉了。
>
> 然而又过了两天，网站又崩了~问题和上面一样，于是又去网上找解决方案，在这篇文章中找到了解决方案（https://www.cnblogs.com/qq78292959/p/3994349.html）
>
> 文章中说道将`config set stop-writes-on-bgsave-error` 设置为no仅仅是让redis忽略了这个异常，使得程序能够继续往下运行，但实际上数据还是会存储到硬盘失败。
>
> 查看redis的日志，会发现一行警告：
>
> `“WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.”`（警告：过量使用内存设置为0！在低内存环境下，后台保存可能失败。为了修正这个问题，请在`/etc/sysctl.conf` 添加一项 `vm.overcommit_memory = 1`，然后重启（或者运行命令'sysctl vm.overcommit_memory=1' ）使其生效。）
>
> 意思是说我系统内存不足，我看了下系统内存 `free -m`，明明还有挺多内存的啊。
>
> 迷迷糊糊中我跟着修改`vm.overcommit_memory=1`后问题果然解决了。有个问题，那明明系统内存还够，为什么redis会认为redis内存不足呢。上面链接中的文章也给出了问题原因分析
>
> redis认为内存不足原因：http://www.linuxidc.com/Linux/2012-07/66079.htm，简单地说：Redis在保存数据到硬盘时为了避免主进程假死，需要Fork一份主进程，然后在Fork进程内完成数据保存到硬盘的操作，如果主进程使用了4GB的内存，Fork子进程的时候需要额外的4GB，此时内存就不够了，Fork失败，进而数据保存硬盘也失败了。
>
> 而将`vm.overcommit_memory`改为1有什么作用呢，网上看到一个博客是如下解释，我个人也比较同意
>
> > 0 — 默认设置。个人理解：当应用进程尝试申请内存时，内核会做一个检测。内核将检查是否有足够的可用内存供应用进程使用；如果有足够的可用内存，内存申请允许；否则，内存申请失败，并把错误返回给应用进程。举个例子，比如1G的机器，A进程已经使用了500M，当有另外进程尝试malloc 500M的内存时，内核就会进行check，发现超出剩余可用内存，就会提示失败。
> > 1 — 对于内存的申请请求，内核不会做任何check，直到物理内存用完，触发OOM杀用户态进程。同样是上面的例子，1G的机器，A进程500M，B进程尝试malloc 500M，会成功，但是一旦kernel发现内存使用率接近1个G(内核有策略)，就触发OOM，杀掉一些用户态的进程(有策略的杀)。
> > 2 — 当 请求申请的内存 >= SWAP内存大小 + 物理内存 * N，则拒绝此次内存申请。解释下这个N：N是一个百分比，根据overcommit_ratio/100来确定，比如overcommit_ratio=50，那么N就是50%。
> > ————————————————
> >
> > 版权声明：本文为CSDN博主「反杀闰土的猹」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> > 原文链接：https://blog.csdn.net/weixin_41866960/article/details/89608666

