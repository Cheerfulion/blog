---
title: 5. Linux常用命令
description: '-'
tags:
  - 通过典型应用快速上手Linux
abbrlink: 28d8585e
date: 2021-03-16 23:26:27
---



## 摘要

![1603168763770](http://blog.cdn.ionluo.cn/blog/1603168763770.png)



## 软件操作命令

![1603168817525](http://blog.cdn.ionluo.cn/blog/1603168817525.png)



## 硬件资源信息

![1603169006756](http://blog.cdn.ionluo.cn/blog/1603169006756.png)

1. `free -m` 用MB单位显示内存信息

2. 最近1分钟、5分钟，15分钟的平均负载，这个值的意义是，单位时间段内CPU活动进程数。当然这个值越大就说明服务器压力越大。一般情况下这个值只要不接近服务器的cpu数量就没有关系，如果服务器cpu数量为8，那么这个值接近8，就说明当前服务器存在一定压力。

   ![1603169437082](http://blog.cdn.ionluo.cn/blog/1603169437082.png)

   

> `-h` 是用人类可读的形式显示

3.  查看**CPU信息**  `cat /proc/cpuinfo`

   > 查看系统版本 `cat /proc/version`

   ![1603255294861](http://blog.cdn.ionluo.cn/blog/1603255294861.png)

`/proc/cpuinfo` 这个文件记录了`cpu`的详细信息。目前市面上的服务器通常都是2颗4核`cpu`，在`linux`看来，它就是8个`cpu`。查看这个文件时则会显示8段类似的信息，而最后一段信息中`processor `: 后面跟的是 `7` 所以查看当前系统有几个`cpu`，我们可以使用这个命令： `grep -c 'processor' /proc/cpuinfo` 而如何看几颗物理`cpu`呢，需要查看关键字 `physical id`, 由于此虚拟机只有一个`cpu`所以并未显示关于 `physical id`的信息。



> 想了解更多可以看这里 [https://www.linuxprobe.com/vmstat-top-sar.html](https://www.linuxprobe.com/vmstat-top-sar.html)

3. `fdisk`

   Linux fdisk是一个创建和维护分区表的程序，它兼容DOS类型的分区表、BSD或者SUN类型的磁盘列表。

   显示当前分区情况：

   ```bash
   # fdisk -l
   
   Disk /dev/sda: 10.7 GB, 10737418240 bytes
   255 heads, 63 sectors/track, 1305 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes
   
     Device Boot   Start     End   Blocks  Id System
   /dev/sda1  *      1     13   104391  83 Linux
   /dev/sda2       14    1305  10377990  8e Linux LVM
   
   Disk /dev/sdb: 5368 MB, 5368709120 bytes
   255 heads, 63 sectors/track, 652 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes
   
   Disk /dev/sdb doesn't contain a valid partition table
   ```

## 文件操作命令

### Linux文件的目录结构

`/` 根目录            `/home` 家目录               `/etc` 配置目录                 `/usr` 用户程序目录               `/temp` 临时目录

> `~`是当前用户的家目录， `/tmp`目录系统会定时清理

#### 文件权限 421



#### 文件搜索查找

`find / -name xxx`  从 `/` 目录开始查找 `xxx` 文件或目录

`which xxx`  从环境变量里面找`xxx`文件

`whereis xxx`  从文件索引中查找文件`xxx`

#### 文件压缩与解压



### 文件基本操作

`cd` 进入目录

`ls` 查看目录下的文件

`touch` 新建文件

`mkdir` 新建文件夹

`rm` 删除文件或者文件夹

`cp` 复制

`mv` 移动

`pwd` 显示当前路径

> 删除文件夹 `rm -r [文件夹名]`， 强制删除`rm -rf [文件夹名]`
>
> 

### 文本编辑神器Vim









## 我的常用命令

1. `ps -aux | grep 'xxx'`

   查看`xxx`的相关进程，然后用`kill -9 [pid]`关闭指定进程，通常计算机卡死的时候可以使用该方式杀掉软件进程。

   > 输入 `| grep` 就看不到最上面的表头了。

   ![1604057428241](http://blog.cdn.ionluo.cn/blog/1604057428241.png)

2. `netstat -tlpn`

   查看当前TCP监听端口，并显示出所在端口对应的运行程序名。通常进入web应用配置或者开发的时候查看服务器开放了哪些端口(`Local Address`)进行访问，是什么程序(`Program name`)开放的。

   ![1604057325539](http://blog.cdn.ionluo.cn/blog/1604057325539.png)

3. `lsof -i:[端口号]`

   查看端口号被谁占用。

   >  `lsof`几乎可以替代`netstat`和`ps`的全部工作， 具体可见https://linux.cn/article-4099-1.html

   ![1604057644910](http://blog.cdn.ionluo.cn/blog/1604057644910.png)