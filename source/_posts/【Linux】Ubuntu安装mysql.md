---
title: Ubuntu安装mysql
description: '-'
tags:
  - Linux
abbrlink: 455e7f2h
date: 2021-05-10 20:21:26
---



# Ubuntu18.04下安装MySQL



## 查看有没有安装过MySQL

```bash
dpkg -l | grep mysql
```

如果安装过的话会有mysql-server，如果没有安装过，那么就继续吧！



## 安装

```bash
apt install mysql-server

# 查看mysql是否安装成功
netstat -tap | grep mysql

# 查看mysql的版本
mysql -V
```



## 安全设置

此时，MySQL的密码为空，可以通过`mysql -u root -p`，遇到密码直接回车就进到数据库了。

接下来，为了确保数据库的安全性和正常运转，对数据库进行初始化操作。这个初始化操作涉及下面5个步骤。

（1）安装验证密码插件。

（2）设置root管理员在数据库中的专有密码。

（3）随后删除匿名账户，并禁止root管理员从远程登录数据库，以确保数据库上运行的业务的安全性。

（4）删除默认的测试数据库，取消测试数据库的一系列访问权限。

（5）刷新授权列表，让初始化的设定立即生效。

```bash
# 这里对于第1点和第3点选择了NO, 视个人情况而定
root@xxx:/var/www/# mysql_secure_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin? # 安装验证密码插件?

Press y|Y for Yes, any other key for No: n
Please set the password for root here. # 设置root管理员在数据库中的专有密码

New password:

Re-enter new password:
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y # 删除匿名账户?
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n # 禁止root远程登录?

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y # 删除test数据库并取消对它的访问权限
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y # 刷新授权表
Success.

All done!
```

此时，就只能通过账号密码登录MySQL了。但是，此时无法通过远程来访问，对于正式环境来说，就无法用图形化工具查看数据库的内容。



## Navicat远程连接

**！！！生产环境不建议开放数据库的远程访问。**

1. 配置MySQL允许远程访问

   ```bash
   vim /etc/mysql/mysql.conf.d/mysqld.cnf
   
   # 注释掉 bind-address = 127.0.0.1
   # 或
   # 修改为：bind-address = 0.0.0.0
   ```

2. 为需要远程登录的用户赋予权限

   ```bash
   # 普通用户远程连接
   grant all on *.* to admin@'%' identified by '123456' with grant option; 
   flush privileges;
   # 允许任何ip地址(%表示允许任何ip地址)的电脑用admin帐户和密码(123456)来访问这个mysql server。
   # 注意admin账户不一定要存在。
   
   # root用户远程连接
   grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
   flush privileges;
   
   # 查看系统用户
   # use mysql;
   # select user,host from user;
   ```

3. 重启MySQL

   ```bash
   systemctl restart mysql
   ```

4. 打开Navicat就可以远程连接上了（如果是生产环境，通常还需要检查安全组是否有开放默认的3306端口）



> Navicat premium破解教程：https://www.52pojie.cn/forum.php?mod=viewthread&tid=1142909
>
> 如果使用上面的方法破解不成功，可以删除Navicat的注册表再重试一次。[彻底删除Navicat](https://blog.csdn.net/kevin1993best/article/details/105642367)
>
> ![image-20210510155838734](http://blog.cdn.ionluo.cn/blog/image-20210510155838734.png)