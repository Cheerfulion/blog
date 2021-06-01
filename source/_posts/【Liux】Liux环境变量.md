---
title: 【Liux】Liux环境变量
tags:
  - Liux
abbrlink: 3e00bca2
date: 2021-05-31 17:30:01
---

 

## linux查看环境变量

1. 使用echo命令查看单个环境变量。

   ```bash
   echo $PATH
   ```

   

2. 使用env查看所有环境变量。

   ```bash
   env
   ```

 

## linux变量的种类

按变量的生存周期来划分，Linux变量可分为两类：
1 永久的：需要修改配置文件，变量永久生效。
2 临时的：使用export命令声明即可，变量在关闭shell时失效。

 

## linux的环境变量相关文件梳理

#### /etc/profile

为系统的每个用户设置环境信息和启动程序，当用户第一次登录时，该文件被执行，其配置对所有登录的用户都有效。当被修改时，必须重启才会生效。英文描述：”System wide environment and startup programs, for login setup.”

#### /etc/environment

系统的环境变量，/etc/profile是所有用户的环境变量，前者与登录用户无关，后者与登录用户有关，当同一变量在两个文件里有冲突时，以用户环境为准。

#### /etc/bashrc

为每个运行 bash shell 的用户执行该文件，当 bash shell 打开时，该文件被执行，其配置对所有使用bash的用户打开的每个bash都有效。当被修改后，不用重启只需要打开一个新的 bash 即可生效。英文描述：”System wide functions and aliases.”

#### ~/.bash_profile

为当前用户设置专属的环境信息和启动程序，当用户登录时该文件执行一次。默认情况下，它用于设置环境变量，并执行当前用户的 .bashrc 文件。理念类似于 /etc/profile，只不过只对当前用户有效，也需要重启才能生效。(注意：Centos7系统命名为.bash_profile，其他系统可能是.bash_login或.profile。)

#### ~/.bashrc

为当前用户设置专属的 bash 信息，当每次打开新的shell时，该文件被执行。理念类似于/etc/bashrc，只不过只对当前用户有效，不需要重启只需要打开新的shell即可生效。

#### ~/.bash_logout

为当前用户，每次退出bash shell时执行该文件，可以把一些清理工作的命令放进这个文件。

#### /etc/profile.d/

此文件夹里是除/etc/profile之外其他的”application-specific startup files”。英文描述为”The /etc/profile file sets the environment variables at startup of the Bash shell. The /etc/profile.d directory contains other scripts that contain application-specific startup files, which are also executed at startup time by the shell.” 同时，这些文件”are loaded via /etc/profile which makes them a part of the bash “profile” in the same way anyway.” 因此可以简单的理解为是/etc/profile的一部分，只不过按类别或功能拆分成若干个文件进行配置了（方便维护和理解）。

>  注意事项: 以上需要重启才能生效的文件，其实可以通过source xxx暂时生效。

 

## linux环境变量文件执行顺序

文件的执行顺序为：当登录Linux时，首先启动/etc/environment和/etc/profile，然后启动当前用户目录下的/.bash_profile，执行此文件时一般会调用/.bashrc文件，而执行/.bashrc时一般会调用/etc/bashrc，最后退出shell时，执行/.bash_logout。简单来说顺序为：（登录时）/etc/environment –> /etc/profile(以及/etc/profile.d/里的文件) –> ~/.bash_profile –> （打开shell时）~/.bashrc –> /etc/bashrc –> （退出shell时）~/.bash_logout

 

## linux设置变量的三种方法

1. 在`/etc/profile`文件中添加变量【对所有用户生效(永久的)】

   推荐使用这种方法，因为所有用户的shell都有权使用这些环境变量，缺点是可能会给系统带来安全性问题。 这里是针对所有的用户, 所有的shell;

   ```bash
   # 可能不存在，若不存在建议加上下面的代码，让环境每次都加载上当前用户的环境变量（显式调用.bashrc）：
   # if [ -f ~/.bashrc ]; then
   #         . ~/.bashrc
   # fi
   sudo vim /etc/profile
   
   # 在文件末尾添加
   # /usr/local/node/bin是需要添加的环境变量路径
   export PATH=$PATH:/usr/local/node/bin
   
   # 保存退出后，运行使环境变量立刻生效
   source /etc/profile
   ```

   > 注：一般只有root用户才有编辑权限；

2. 在用户目录下的`.bash_profile`文件中增加变量【对单一用户生效(永久的)】
   例如：编辑ion用户目录(/home/ion)下的`.bash_profile`

   ```bash
   vim /home/ion/.bash_profile
   # 在文件末尾添加
   export PATH=$PATH:/usr/local/node/bin
   # 保存退出后，运行使环境变量立刻生效
   source /home/ion/.bash_profile
   ```

3. 在用户目录下的`.bashrc`文件中增加变量【对单一用户生效(永久的)】

   例如：编辑ion用户目录(/home/ion)下的`.bashrc`

   ```bash
   vim /home/ion/.bashrc
   # 在文件末尾添加
   export PATH=$PATH:/usr/local/node/bin
   # 保存退出后，运行使环境变量立刻生效
   source /home/ion/.bashrc
   ```

   > 方法2和3是针对单一用户的，永久有效的，它可以把使用这些环境变量的权限控制到用户级别,这里是针对某一个特定的用户，所以更安全。
   >
   > .bashrc在用户登录和打开新的shell时加载，.bash_profile在用户登录时仅加载一次。

4. 直接运行export命令定义变量【只对当前shell(BASH)有效(临时的)】

   ```bash
   export PATH=$PATH:/usr/local/node/bin
   ```

   > 该方法的PATH 在终端关闭 后就会消失。

 

## 参考

- [linux查看、添加、删除环境变量](https://blog.csdn.net/mayue_web/article/details/97023615)

- [Linux 之 .bashrc 文件作用](https://www.cnblogs.com/midworld/p/11006967.html)

- [Linux下环境变量配置方法梳理](https://blog.csdn.net/uisoul/article/details/89439575)