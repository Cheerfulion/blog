---
title: ubuntu安装报错问题汇总
description: 暂无描述！
tags:
  - 运维
  - Linux
abbrlink: '39014265'
date: 2021-03-06 12:57:00
---



### ubuntu提示E: 无法获得锁 /var/lib/dpkg/lock-frontend - open (11: 资源暂时不可用)

参考：https://blog.csdn.net/dream_follower/article/details/90311799

**原因:**
出现这个问题的原因可能是有另外一个程序正在运行，由于它在运行时，会占用软件源更新时的系统锁（以下称“系统更新锁”，此锁文件在“/var/lib/apt/lists/”目录下），而当有新的apt-get进程生成时，就会因为得不到系统更新锁而出现”E: 无法获得锁 /var/lib/apt/lists/lock - open (11: Resource temporarily unavailable)”错误提示！
而导致资源被锁的原因，可能是上次安装时没正常完成，而导致出现此状况。
因此，我们只要将原先的apt-get进程杀死，从新激活新的apt-get进程，就可以让软件管理器正常工作了！

**解决方案：**
1. 方法一：
用这个命令查看一下`apt-get`的相关进程：

```shell
ion@ubuntu:$ ps -e | grep apt
  7700 ?        00:00:00 apt.systemd.dai
  7714 ?        00:00:00 apt.systemd.dai
  8291 ?        00:00:01 aptd
```

然后执行:

```
ion@ubuntu:$ sudo kill -9 7700 7714 8291
```

用了上面方法，如果还不行的话，继续执行方法2
2.方法二：

```
sudo rm /var/lib/dpkg/lock-frontend 
sudo rm /var/cache/apt/archives/lock  
sudo rm /var/lib/dpkg/lock  
```