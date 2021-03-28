---
title: Mysql使用过程遇到的问题记录
description: 暂无描述！
tags:
  - 数据库
  - mysql
abbrlink: c599ad35
date: 2021-03-06 12:57:00
---



## [解决远程连接mysql很慢的方法](https://www.cnblogs.com/shenyixin/p/10478604.html)

开发某应用系统连接公司的测试服务器的mysql数据库连接打开的很慢，但是连接本地的mysql数据库很快。但是测试站点打开速度正常，ping访问正常。



解决方法：

找到mysql配置文件, 增加配置参数：

```bash
[mysqld]
skip-name-resolve
```

注意该配置项是加到`mysqld`下的，该配置的作用是**避免mysql对外部的连接进行DNS解析，若使用此设置，那么远程主机连接时只能使用ip，而不能使用域名**（但是我这里还是可以使用域名，可能是因为公司内的这个域名是内部连接）

> Linux下配置文件一般在 `/etc/my.cnf` 或者 `/etc/mysql/my.cnf`, Windows下配置文件是mysql安装目录下的`my.ini`文件
>
> 修改配置文件后需要重启mysql



[其他优化配置建议](https://blog.csdn.net/enweitech/article/details/52381612)：

```
[mysqld]
max_allowed_packet=16M 
增加该变量的值十分安全，这是因为仅当需要时才会分配额外内存。例如，仅当你发出长查询或mysqld必须返回大的结果行时mysqld才会分配更多内存。该变量之所以取较小默认值是一种预防措施，以捕获客户端和服务器之间的错误信息包，并确保不会因偶然使用大的信息包而导致内存溢出。 
如果你正是用大的BLOB值，而且未为mysqld授予为处理查询而访问足够内存的权限，也会遇到与大信息包有关的奇怪问题。如果怀疑出现了该情况，请尝试在mysqld_safe脚本开始增加ulimit -d 256000，并重启mysqld。 
##########################################
#####   MySQL怎样打开和关闭数据库表 #####
##########################################
table_cache, max_connections和max_tmp_tables影响服务器保持打开的文件的最大数量。如果你增加这些值的一个或两个，你可以遇到你的操作系统每个进程打开文件描述符的数量上强加的限制。然而，你可以能在许多系统上增加该限制。请教你的OS文档找出如何做这些，因为改变限制的方法各系统有很大的不同。 
table_cache与max_connections有关。例如，对于200个打开的连接，你应该让一张表的缓冲至少有200 * n，这里n是一个联结(join)中表的最大数量。
打开表的缓存可以增加到一个table_cache的最大值（缺省为64；这可以用mysqld的-O table_cache=#选项来改变）。一个表绝对不被关闭，除非当缓存满了并且另外一个线程试图打开一个表时或如果你使用mysqladmin refresh或mysqladmin flush-tables。 
当表缓存满时，服务器使用下列过程找到一个缓存入口来使用： 
不是当前使用的表被释放，以最近最少使用（LRU）顺序。 
如果缓存满了并且没有表可以释放，但是一个新表需要打开，缓存必须临时被扩大。 
如果缓存处于一个临时扩大状态并且一个表从在用变为不在用状态，它被关闭并从缓存中释放。 
对每个并发存取打开一个表。这意味着，如果你让2个线程存取同一个表或在同一个查询中存取表两次(用AS)，表需要被打开两次。任何表的第一次打开占2个文件描述符；表的每一次额外使用仅占一个文件描述符。对于第一次打开的额外描述符用于索引文件；这个描述符在所有线程之间共享

skip-networking

不在tcp/ip端口上进行监听，所有的连接都是通过本地的socket文件连接，这样可以提高安全性，确定是不能通过网络连接数据库。

skip-locking

避免mysql的外部锁定，增强稳定性
skip-name-resolve

避免mysql对外部的连接进行DNS解析，若使用此设置，那么远程主机连接时只能使用ip，而不能使用域名

max_connections = 3000

指定mysql服务所允许的最大连接进程数，

max_connect_errors = 1000

每个主机连接允许异常中断的次数，当超过该次数mysql服务器将禁止该主机的连接请求，直到mysql服务重启，或者flushhosts命令清空host的相关信息

table_cache = 614k

表的高速缓冲区的大小，当mysql访问一个表时，如果mysql表缓冲区还有空间，那么这个表就会被打开通放入高速缓冲区，好处是可以更快速的访问表中的内容。
如果open_tables和opened_tables的值接近该值，那么久该增加该值的大小

max_allowed_packet = 4M

设定在网络传输中一次可以传输消息的最大值，系统默认为1M，最大可以是1G

sort_buffer_size = 16M

排序缓冲区用来处理类似orderby以及groupby队列所引起的排序，系统默认大小为2M,该参数对应分配内存是每个连接独占的，若有100个连接，实际分配的排序缓冲区大小为6*100；推荐设置为6M-8M

join_buffer_size 8M

联合查询操作所使用的缓冲区大小。

thread_cache_size = 64


设置threadcache池中可以缓存连接线程的最大数量，默认为0，该值表示可以重新利用保存在缓存中线程的数量，当断开连接时若缓存中还有空间，那么客户端的线程将被放到缓存中，如果线程重新被请求，那么请求将从缓存中读取，若果缓存中是空的或者是新的请求，那么线程将被重新创建。设置规律为：1G内存设置为8,2G内存设置为16,4G以上设置为64

query_cache_size = 64M

指定mysql查询缓冲区的大小，用来缓冲select的结果，并在下一次同样查询的时候不再执行查询而直接返回结果，根据Qcache_lowmem_prunes的大小，来查看当前的负载是否足够高

query_cache_limit = 4M

只有小于该值的结果才被缓冲，放置一个极大的结果将其他所有的查询结果都覆盖
tmp_table_size 256M

内存临时表的大小，如果超过该值，会将临时表写入磁盘

default_storage_engine = MYISAM

创建表时默认使用的存储引擎

log-bin=mysql-bin

打开二进制日志功能

key_buffer_size = 384M

指定索引缓冲区的大小，内存为4G时刻设置为256M或384M

read_buffer_size = 8M

用来做MYISAM表全表扫描的缓冲大小
```





## 无法启动Mysql

报错截图：

![image-20210225180336117](http://blog.cdn.ionluo.cn/blog/image-20210225180336117.png)



![image-20210225180345817](http://blog.cdn.ionluo.cn/blog/image-20210225180345817.png)



![image-20210225180355586](http://blog.cdn.ionluo.cn/blog/image-20210225180355586.png)



解决：

这里是根据最后一点日志中的报错 `mysqld: Can't create/write to file '/tmp/ibo4T3D4' (Errcode: 13 - Permission denied)`解决的。

1. top查看mysql的用户是什么？

   ```bash
   root@ip:/# top | grep mysql
   ```

   

   ![image-20210225180717620](http://blog.cdn.ionluo.cn/blog/image-20210225180717620.png)

2.  修改mysql的临时目录（这里可以不修改，但是不修改直接给`/tmp`赋权限我感觉不安全）

   找到mysql的配置文件，我的位置在 `/etc/mysql/my.cnf`

   ```bash
   vim /etc/mysql/my.cnf
   
   # 修改tmpdir
   tmpdir = /tmpdir/mysql
   ```



3. 给新的mysql临时目录赋予权限

   ```bash
   # 设置目录/tmpdir/mysql的文件所有者和文件关联组
   chown -R mysql:mysql /tmpdir/mysql
   # 下面的是以防万一，给所有用户和组赋予所有权限，可以不加
   chmod -R 777 /tmpdir/mysql
   ```

   > mysql 是上面top查看到的USER名称， 这里因为是临时文件，我感觉没有必要区分用户，直接就给了所有权限

   

4. 重启mysql

   ```bash
   service mysql stop
   service mysql start
   # 或者
   service mysql restart
   ```

   如果重启还是失败，直接给 `/tmp` 777 的权限吧，暴力直接，但是不清楚会不会有其他后果。我这里就遇到过这个问题，就只能设置成 `/tmp` 目录才不会报错，原因不明，等待大佬给我答疑解惑。

