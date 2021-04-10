---
title: scp断点续传
description: '-'
tags:
  - 运维
  - Linux
abbrlink: 59a8f40b
date: 2021-02-12 13:31:15
---



## SCP

**从 本地 复制到 远程**

```bash
scp local_file remote_username@remote_ip:[remote_folder|remote_file]
```

**从 远程 复制到 本地**

```bash
scp remote_username@remote_ip:[remote_folder|remote_file] local_file 
```



具体可以参考：https://www.runoob.com/linux/linux-comm-scp.html



## scp断点续传

scp 是通过ssh协议传输数据，如果是想传输一个很大的数据，有可能遇到连接中断等问题又要重新上传的悲剧。通过类似`scp`拷贝的另一个命令`rsync`就可以实现意外中断后，下次继续传输，命令如下：

```bash
rsync -P --rsh=ssh local_file remote_username@remote_ip:remote_folder
```

> -P: 是包含了 `–partial –progress`， 部分传送和显示进度
>
> -rsh=ssh 表示使用ssh协议传送数据



```bash
# 这里举个例子，从远程拷贝数据库信息到本地数据库

# 注意需要提前在远程生成好数据库文件
# mysqldump -uroot -p xxx_com|gzip > /var/www/xxx_com.sql.gz

rsync -P --rsh=ssh root@www.xxx.com:/var/www/xxx_com/xxx_com.sql.gz ./

gzip -df xxx_com.sql.gz
mysql -hlocalhost -uroot -p --database=xxx_com < xxx_com.sql
```

