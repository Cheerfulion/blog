---
title: scp断点续传
description: 暂无描述！
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

