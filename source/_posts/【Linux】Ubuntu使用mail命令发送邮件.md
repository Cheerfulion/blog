---
title: Ubuntu使用mail命令发送邮件
description: '-'
tags:
  - 运维
  - Linux
abbrlink: abdd27f8
date: 2021-03-31 20:15:42
---



## Ubuntu 16.04 使用mailutils发送邮件

1. 安装`mailutils`

   ```bash
   sudo apt install mailutils
   ```

   ![1607566393707](http://blog.cdn.ionluo.cn/blog/1607566393707.png)

   ![1607566559110](http://blog.cdn.ionluo.cn/blog/1607566559110.png)

   ![1607566631213](http://blog.cdn.ionluo.cn/blog/1607566631213.png)

2. 安装`heirloom-mailx`

   ```bash
   sudo apt-get install heirloom-mailx
   ```

3. 配置`heirloom-mailx`

   ```bash
   # sudo nano /etc/nail.rc
   
   set from=USER@qq.com  # 发送人的邮件地址，一般就是你的邮箱账号
   set smtp=smtp.qq.com  # 外部smtp服务器的地址，不同邮箱不一样，如163邮箱的是smtp.163.com
   set smtp-auth-user=USER@qq.com  # 外部smtp服务器认证的用户名（账号）
   set smtp-auth-password=PASSWORD  # 外部smtp服务器认证的用户密码或者授权码
   set smtp-auth=login  #认证方式
   ```

   > 其中`USER@qq.com`是你的邮箱账号,PASSWORD是你邮箱的密码或者授权码.
   >
   > ![1607568084978](http://blog.cdn.ionluo.cn/blog/1607568084978.png)
   >
   > qq邮箱的授权码需要开启SMTP服务（安装mailutils就是选择的这个服务），至于IMAP和POP3的区别可以看官方说明。

4. 接下来就可以通过mail或者命令发送邮件了

   ```bash
   echo "邮件内容" | heirloom-mailx -s "邮件标题" ionluo@cyagen.com
   # 和下面是一样的
   echo "邮件内容" | mail -s "邮件标题" ionluo@cyagen.com
   
   # 加参数v可以看到mail输出的详细(Verbose)信息:
   echo "邮件内容" | heirloom-mailx -vs "邮件标题" ionluo@cyagen.com
   # 和下面是一样的
   echo "邮件内容" | mail -vs "邮件标题" ionluo@cyagen.com
   ```

   

**总结：**  `/bin/mail`会默认使用本地`sendmail`发送邮件，这样要求本地的机器必须安装和启动`Sendmail`服务，配置非常麻烦，而且会带来不必要的资源占用。而通过修改配置文件可以使用外部SMTP服务器，可以达到不使用`sendmail`而用外部的`smtp`服务器发送邮件的目的.



## 问题

1. 安装了其他软件包需要卸载

   > apt-get的卸载相关的命令有remove/purge/autoremove/clean/autoclean等。具体来说：
   >
   > **apt-get purge / apt-get --purge remove**
   > 删除已安装包（不保留配置文件)。
   > 如软件包a，依赖软件包b，则执行该命令会删除a，而且不保留配置文件
   >
   > **apt-get autoremove**
   > 删除为了满足依赖而安装的，但现在不再需要的软件包（包括已安装包），保留配置文件。
   >
   > **apt-get remove**
   > 删除已安装的软件包（保留配置文件），不会删除依赖软件包，且保留配置文件。
   >
   > **apt-get autoclean**
   > APT的底层包是dpkg, 而dpkg 安装Package时, 会将 *.deb 放在 /var/cache/apt/archives/中，apt-get autoclean 只会删除 /var/cache/apt/archives/ 已经过期的deb。
   >
   > **apt-get clean**
   > 使用 apt-get clean 会将 /var/cache/apt/archives/ 的 所有 deb 删掉，可以理解为 rm /var/cache/apt/archives/*.deb。
   >
   > ------
   >
   > 那么如何彻底卸载软件呢？
   > 具体来说可以运行如下命令：
   >
   > ```bash
   > # 删除软件及其配置文件
   > apt-get --purge remove <package>
   > # 删除没用的依赖包（危险操作：https://www.zhihu.com/question/392688534）
   > # apt-get autoremove <package>
   > # 此时dpkg的列表中有“rc”状态的软件包，可以执行如下命令做最后清理：
   > dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P123456
   > ```
   >
   > 当然如果要删除暂存的软件安装包，也可以再使用clean命令。

2. `Package heirloom-mailx is not available, but is referred to by another package.`
   `This may mean that the package is missing, has been obsoleted, or`
   `is only available from another source`

   `E: Package 'heirloom-mailx' has no installation candidate`

   需要更新软件源（注：Ubuntu16.04）。

   ```bash
   # 首先备份源列表
   sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup_20201210
   
   # 而后打开sources.list文件修改
   sudo vim /etc/apt/sources.list
   
   # 用下面阿里源替换掉文件中所有的内容，保存编辑好的文件
   deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse 
   deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse 
   deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse 
   deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse 
   deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse 
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse 
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse 
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse 
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse 
   deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
   
   # 刷新列表，注意一定要执行刷新
   sudo apt-get update
   # 上面执行后就更新好了，下面是升级包和安装C/C++的编译环境
   # sudo apt-get upgrade
   # sudo apt-get install build-essential
   ```

3. `nano`编辑器使用

   具体可以看这个https://blog.csdn.net/u010359398/article/details/80944403

   大多数都可以看底部提示知道，这里说下常用编辑操作。

   编辑后，`ctrl + X` 退出，提示保存格式，直接`Enter`即可保存。

4. 阿里云企业邮箱作为外部邮件服务器

   https://developer.aliyun.com/ask/182907?spm=a2c6h.13524658

   因为在企业邮箱中没有找到这个设置，查了下原来默认是开启的，外部服务器是`smtp.mxhichina.com`， 端口是`25`, 账号，密码即可登录了。

   配置如下：

   ```bash
   # sudo vim /etc/s-nail.rc
   set from=ionluo@xxx.com
   set smtp=smtp.mxhichina.com
   set smtp-auth-user=ionluo@xxx.com
   set smtp-auth-password=xxxxxx
   set smtp-auth=login
   ```

   

