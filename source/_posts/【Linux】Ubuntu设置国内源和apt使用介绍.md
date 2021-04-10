---
title: Ubuntu设置国内源和apt使用介绍
description: '-'
tags:
  - 运维
  - Linux
abbrlink: b191bda3
date: 2021-03-31 20:15:42
---



## 配置阿里源

1、复制原文件备份

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

2、编辑源列表文件

```bash
sudo vim /etc/apt/sources.list
```

3、**文件头部添加**添加如下内容

> 注意不要删除原来的，防止有些库阿里源上找不到

```
deb http://mirrors.aliyun.com/ubuntu/ vivid main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ vivid-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ vivid-backports main restricted universe multiverse
```

4. 同步 `/etc/apt/sources.list` 和 `/etc/apt/sources.list.d` 中列出的源的索引

```bash
sudo apt-get update

# 升级所有已安装的软件包
# sudo apt-get upgrade
```

> `sources.list 文件`里面保存的是官方的软件源，你可以任一选择阿里、网易等国内的源复制粘贴进去，保存。
>
> `sources.list.d 目录` 下的文件是第三方软件的源，可以分别存放不同的第三源地址，只需扩展名为list即可。



## apt-get 常用命令

```bash
apt-cache search packagename 搜索包
# apt-cache show packagename 获取包的相关信息，如说明、大小、版本等
# apt-get install packagename 安装包
# apt-get install packagename - - reinstall 重新安装包
apt-get -f install 修复安装"-f = --fix-missing"
# apt-get remove packagename 删除包
# apt-get remove packagename - - purge 删除包，包括删除配置文件等
# apt-get update 更新源
apt-get upgrade 更新已安装的包
apt-get dist-upgrade 升级系统
apt-get dselect-upgrade 使用 dselect 升级
apt-cache depends packagename 了解使用依赖
apt-cache rdepends packagename 是查看该包被哪些包依赖
apt-get build-dep packagename 安装相关的编译环境
apt-get source packagename 下载该包的源代码
apt-get clean 清理无用的包
apt-get autoclean 清理无用的包
apt-get check 检查是否有损坏的依赖
```



## apt-get的卸载相关的命令详细说明

apt-get的卸载相关的命令有remove/purge/autoremove/clean/autoclean等。具体来说：

**apt-get purge / apt-get --purge remove**
删除已安装包（不保留配置文件)。
如软件包a，依赖软件包b，则执行该命令会删除a，而且不保留配置文件

**apt-get autoremove**
删除为了满足依赖而安装的，但现在不再需要的软件包（包括已安装包），保留配置文件。

**apt-get remove**
删除已安装的软件包（保留配置文件），不会删除依赖软件包，且保留配置文件。

**apt-get autoclean**
APT的底层包是dpkg, 而dpkg 安装Package时, 会将 *.deb 放在 /var/cache/apt/archives/中，apt-get autoclean 只会删除 /var/cache/apt/archives/ 已经过期的deb。

**apt-get clean**
使用 apt-get clean 会将 /var/cache/apt/archives/ 的 所有 deb 删掉，可以理解为 rm /var/cache/apt/archives/*.deb。

------

那么如何彻底卸载软件呢？
具体来说可以运行如下命令：

```bash
# 删除软件及其配置文件
apt-get --purge remove <package>
# 删除没用的依赖包（危险操作：https://www.zhihu.com/question/392688534）
# apt-get autoremove <package>
```

当然如果要删除暂存的软件安装包，也可以再使用clean命令。



## apt-get 工作原理

Ubuntu采用集中式的软件仓库机制，将各式各样的软件包分门别类地存放在软件仓库中，进行有效地组织和管理。然后，将软件仓库置于许许多多的镜像服务器中，并保持基本一致。这样，所有的Ubuntu用户随时都能获得最新版本的安装软件包。因此，对于用户，这些镜像服务器就是他们的软件源（Reposity）

然而，由于每位用户所处的网络环境不同，不可能随意地访问各镜像站点。为了能够有选择地访问，在Ubuntu系统中，使用软件源配置文件/etc/apt/sources.list列出最合适访问的镜像站点地址。

1. **apt-get 常用命令**

> 1）执行apt-get update
> 2）程序分析/etc/apt/sources.list
> 3）自动连网寻找list中对应的Packages/Sources/Release列表文件，如果有更新则下载之，存入/var/lib/apt/lists/目录
> 4）然后 apt-get install 相应的包 ，下载并安装。

即使这样，软件源配置文件只是告知Ubuntu系统可以访问的镜像站点地址，但那些镜像站点具体都拥有什么软件资源并不清楚。若每安装一个软件包，就在服务器上寻找一遍，效率是很低的。**因而，就有必要为这些软件资源列个清单（建立索引文件），以便本地主机查询。**

用户可以使用“apt-get update”命令刷新软件源，建立更新软件包列表。在Ubuntu Linux中，**“apt-get update”命令会扫描每一个软件源服务器，并为该服务器所具有软件包资源建立索引文件，存放在本地的/var/lib/apt/lists/目录中。** 使用apt-get执行安装、更新操作时，都将依据这些索引文件，向软件源服务器申请资源。因此，在计算机设备空闲时，经常使用“apt-get update”命令刷新软件源，是一个好的习惯。

2. **apt-get install原理**

 

![img](http://blog.cdn.ionluo.cn/blog/1446087-28364955d235f67c.png)





## 参考

https://www.cnblogs.com/clemente/p/10688169.html