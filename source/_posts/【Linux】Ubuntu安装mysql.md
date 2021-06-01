---
title: Ubuntu安装mysql
description: '-'
tags:
  - Linux
abbrlink: 455e7f2h
date: 2021-05-10 20:21:26
---



## 前言

Ubuntu18.04下安装MySQL和phpMyAdmin



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

   Navicat premium破解教程：https://www.52pojie.cn/forum.php?mod=viewthread&tid=1142909

   如果使用上面的方法破解不成功，可以删除Navicat的注册表再重试一次。[彻底删除Navicat](https://blog.csdn.net/kevin1993best/article/details/105642367)

   ![image-20210510155838734](http://blog.cdn.ionluo.cn/blog/image-20210510155838734.png)



## 搭建phpMyAdmin实现生产环境的数据库访问

[PhpMyAdmin](https://www.phpmyadmin.net/) 是一个用 PHP 编写的软件工具，可以通过 web方式控制和操作 MySQL 或 MariaDB 数据库。通过 phpMyAdmin 可以完全对数据库进行操作，例如建立、复制和删除数据等等，这样 数据库的管理就会变得相当简单。

**下载phpMyAdmin文件安装（推荐）：**

```bash
# mysql -V
mysql  Ver 14.14 Distrib 5.7.33, for Linux (x86_64) using  EditLine wrapper
# php -v
PHP 7.2.24-0ubuntu0.18.04.7 (cli) (built: Oct  7 2020 15:24:25) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.24-0ubuntu0.18.04.7, Copyright (c) 1999-2018, by Zend Technologies
# nginx -v
nginx version: nginx/1.14.0 (Ubuntu)
# lsb_release -a
LSB Version:    core-9.20170808ubuntu1-noarch:security-9.20170808ubuntu1-noarch
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.5 LTS
Release:        18.04
Codename:       bioni

# 上面列出了安装的环境

# 安装PHP（nginx使用php的话要用到php7.2-fpm,所以要安装）
sudo apt-get install php7.2-mysql php7.2-fpm php7.2-curl php7.2-xml php7.2-gd php7.2-mbstring php-memcached php7.2-zip

# 配置php-fpm(https://www.zhaokeli.com/article/8496.html#mulu3)
# 这里就采用sock文件的方式，修改其权限
chmod 777 /run/php/php7.2-fpm.sock

# 启动php7.2-fpm
sudo service php7.2-fpm start 
# 重启命令是：sudo service php7.2-fpm restart

# 修改nginx配置文件, 增加配置文件phpmyadmin.cnf
vim /etc/nginx/sites-enabled/phpmyadmin

# 加入下面代码
server {
        listen   80; #监听80端口，接收http请求
        server_name  mysql.xxx.com; # 网站地址(这里写一个域名，国内需要备案并设置好域名解析。也可以不设置，通过ip来访问，但是很有可能会出现端口冲突的问题，有条件的情况下尽量设置域名)
        root   /var/www/phpmyadmin;  # 存放代码工程的路径(后面步骤将phpmyadmin放在这个位置)
        index  index.php index.html index.htm index.nginx-debian.html;

        access_log /var/log/nginx/phpmyadmin_access.log;
        error_log /var/log/nginx/phpmyadmin_error.log;
        
        location / {
                try_files $uri $uri/ =404;
        }
        
        location ~ /\.ht {
                deny all;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass   unix:/run/php/php7.2-fpm.sock
        }
}

# 去官网下载支持PHP版本的ZIP包（https://www.phpmyadmin.net/news/）
wget https://files.phpmyadmin.net/phpMyAdmin/4.8.5/phpMyAdmin-4.8.5-all-languages.zip

# 解压
unzip phpMyAdmin-4.8.5-all-languages.zip

# 移动至项目目录
mv phpMyAdmin-4.8.5-all-languages /var/www/phpmyadmin

# 设置配置文件
cd /var/www/phpmyadmin
cp config.sample.inc.php config.inc.php

# 修改配置文件
vim config.inc.php
# YOU MUST FILL IN THIS FOR COOKIE AUTH!
$cfg['blowfish_secret'] = 'i am ionluo, who you are?';
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['host'] = 'localhost';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['compress'] = false;
$cfg['Servers'][$i]['AllowNoPassword'] = false;
```

这时候浏览器访问`http://mysql.xxx.com`就可以看到如下页面了。

![image-20210512132903587](http://blog.cdn.ionluo.cn/blog/image-20210512132903587.png)



**使用apt安装（了解即可，不推荐）：**

该方式会安装`apache2`，在服务器上如果有`nginx`的话还是推荐用上面的方式，减少程序的复杂性。

```bash
# 安装phpmyadmin
sudo apt install phpmyadmin
# 安装必要的依赖包（未确认，可能也不需要，这里网上的解释是：因为默认安装phpmyadmin时会安装apahce和php等依赖包，由于是16.04系统，会默认安装php7.0，php7.0又没有默认自带php-mbstring，php-gettext这两个包）
sudo apt-get install php-mbstring
sudo apt-get install php-gettext

 # 这里用apache2提供web服务(apache2)
 ┌────────────────────────────────┤ Configuring phpmyadmin ├────────────────────────────────┐   
 │ Please choose the web server that should be automatically configured to run phpMyAdmin.  │   
 │                                                                                          │   
 │ Web server to reconfigure automatically:                                                 │   
 │                                                                                          │   
 │    [*] apache2                                                                           │   
 │    [ ] lighttpd                                                                          │   
 │                                                                                          │                      
 └──────────────────────────────────────────────────────────────────────────────────────────┘
 
 # 需要配置数据库以使用phpmyadmin (yes)
 ┌─────────────────────────────────┤ Configuring phpmyadmin ├──────────────────────────────────┐  
 │                                                                                             │  
 │ The phpmyadmin package must have a database installed and configured before it can be       │  
 │ used.  This can be optionally handled with dbconfig-common.                                 │  
 │                                                                                             │  
 │ If you are an advanced database administrator and know that you want to perform this        │  
 │ configuration manually, or if your database has already been installed and configured, you  │  
 │ should refuse this option.  Details on what needs to be done should most likely be          │  
 │ provided in /usr/share/doc/phpmyadmin.                                                      │  
 │                                                                                             │  
 │ Otherwise, you should probably choose this option.                                          │  
 │                                                                                             │  
 │ Configure database for phpmyadmin with dbconfig-common?                                     │  
 │                                                                                             │  
 │                          <Yes>                             <No>                             │  
 │                                                                                             │  
 └─────────────────────────────────────────────────────────────────────────────────────────────┘
 
 # 设置phpmyadmin的密码
 ┌────────────────────────────────┤ Configuring phpmyadmin ├────────────────────────────────┐
 │ Please provide a password for phpmyadmin to register with the database server. If left   │
 │ blank, a random password will be generated.                                              │
 │                                                                                          │
 │ MySQL application password for phpmyadmin:                                               │
 │                                                                                          │
 │ ________________________________________________________________________________________ │
 │                                                                                          │
 │                         <Ok>                             <Cancel>                        │
 │                                                                                          │
 └──────────────────────────────────────────────────────────────────────────────────────────┘
```

> 查看安装的文件：`dpkg -L phpmyadmin`
>
> 配置文件：`/etc/phpmyadmin`
>
> 项目目录：`/usr/share/phpmyadmin`
>
> 
>
> 如果是在本地使用，并且apache的80端口未被占用，可以使用下面方法启动phpmyadmin
>
> ```bash
> # 设置软链接
> sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
> # 修改php配置文件（注意版本不一定和这里一样是7.2）
> sudo vim /etc/php/7.2/apache2/php.ini
> # display_errors = On(显示错误日志，出现两次，都要改，不然无效)
> # extension=php_mbstring.dll (开启mbstring)
> # 重启apache
> sudo /etc/init.d/apache2 restart
> ```
>
> 此时，浏览器输入`http://localhost/phpmyadmin`就可以访问了。







## phpmyadmin安装问题记录

1. 没有找到 PHP 扩展 mbstring，而您现在好像在使用多字节字符集。没有 mbstring 扩展的 phpMyAdmin 不能正确分割字符串，可能产生意想不到的结果。

   ![image-20210512100713327](http://blog.cdn.ionluo.cn/blog/image-20210512100713327.png)

   ```bash
   sudo apt-get install php-mbstring
   # 可以通过whereis php来找到php的安装位置
   vim /etc/php/7.2/cli/php.ini
   # ;extension=php_mbstring.dll 添加下面一行
   extension=mbstring.so
   ```

2. 变量 $cfg['TempDir'] （./tmp/）无法访问。phpMyAdmin无法缓存模板文件，所以会运行缓慢。

   ![phpMyAdmin无法缓存模板文件](http://blog.cdn.ionluo.cn/blog/phpMyAdmin无法缓存模板文件.png)

   ```bash
   # 进入phpmyadmin的安装目录
   cd /var/www/phpmyadmin
   # 创建缓存目录文件夹
   mkdir tmp
   # 给文件夹赋予所有权限
   chmod 777 tmp
   ```

3. 配置文件现在需要一个短语密码。

   ```bash
   vim /var/www/phpmyadmin/config.inc.php
   
   # $cfg['Servers'][$i]['auth_type'] = 'cookie';的时候必须提供该短语密码
   $cfg['blowfish_secret'] = 'i am ionluo, you are abcdefghijklmnopqrstuvwxyzabcdefgh';
   ```

4. 使用mysql的账号密码无法登录。

   - 进入mysql的配置文件目录，打开debian.cnf文件

     `vim /etc/mysql/debian.cnf`

   - 使用client里面的账号密码登录phpmyadmin, 找到“账户”→“新增用户账户”

     ![image-20210512133522944](http://blog.cdn.ionluo.cn/blog/image-20210512133522944.png)

   - 新增一个主机名是本地，具有全局权限的账户即可登录。

     > 如果mysql命令行可以登录的话，也可以直接使用mysql命令去修改账户的信息, 同理也可以添加，这里不赘述了。
>
     > ```bash
     > # mysql -u[用户名] -p -P [端口，默认3306]
     > mysql> use mysql;
     > mysql> select host,user,authentication_string from user;
     > mysql> update mysql.user set authentication_string=password('123456') where user='root' and host='localhost';
     > mysql> flush privileges;
     > mysql> quit
     > ```
   
   

   5. MySQL不需要密码就能登录问题

      因为执行了一个更改数据库root用户密码的命令，当我更改完后，发现用我新密码和旧密码都能登陆，于是感觉没有输密码，直接回车就能登录，而我在配置中也没有进行免密码登陆的操作，最后，执行了一条命令解决`update user set plugin = "mysql_native_password";`
   
      修改密码及解决无密码登陆问题都在下面命令中：
   
      ```mysql
      # 无密码即可登录，如mysql, mysql -uroot, mysql -uroot -p 之后不输入密码直接回车
      mysql -u[用户名] -p -P [端口，默认3306]
      
      # 无password字段的版本,也就是版本<=5.7的
      update mysql.user set authentication_string=password("你的密码") where user='root';  
      # 有password字段的版本,版本>5.7的
      update mysql.user set password=password('你的密码') where user='root';
      
      update mysql.user set plugin="mysql_native_password"; 
      
      flush privileges;
      
      exit;
      
      # 重启mysql服务
      service mysql restart
      ```
   
      > 如果上面方式不生效，可以检查下数据库是否存在空用户 ： https://blog.csdn.net/qq_35180983/article/details/82417448



## 参考文章

[ubuntu安装php7.2，php-fpm](https://www.zhaokeli.com/article/8496.html)

[Nginx+PHP+MySQL+phpMyAdmin 环境搭建与使用（12.04.4 LTS）](https://blog.csdn.net/duyiwuer2009/article/details/44966841?locationNum=8)

https://www.cnblogs.com/youcong/p/10703478.html

[解决MySQL不需要密码就能登录问题](https://www.cnblogs.com/youpeng/p/11905051.html)