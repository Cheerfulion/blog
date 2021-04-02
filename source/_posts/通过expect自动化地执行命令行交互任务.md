---
title: 通过expect自动化地执行命令行交互任务
description: '-'
tags:
  - 运维
  - Linux
abbrlink: bda874eb
date: 2021-03-31 20:15:42
---



## Ubuntu 安装 expect

```bash
sudo apt install expect
```



## 学习 expect

不得不说，百度很久都找不到expect的详细文档，都是一些重复的基础用法，这里记录下我找到的expect的中文教程：

http://xstarcd.github.io/wiki/shell/expect.html

https://www.cnblogs.com/TDXYBS/p/11012089.html

## shell结合expect写的批量scp脚本工具

> expect用于自动化地执行linux环境下的命令行交互任务，例如scp、ssh之类需要用户手动输入密码然后确认的任务。有了这个工具，定义在scp过程中可能遇到的情况，然后编写相应的处理语句，就可以自动地完成scp操作了。

```shell
#!/bin/bash

# 这里拷贝的是后台每天自动备份的数据库文件，
# 如果复制失败可能是当天备份失败了，那么修改下面的slug成前几天的日期，如现在的20201124
SLUG=$(date +%Y%m%d)
BACKUP_BASE_DIR="/mnt/backup/xxx_com_databases"
BACKUP_DIR=$BACKUP_BASE_DIR/$SLUG/db.tar.gz
UNZIP_FILE=.$BACKUP_BASE_DIR/$SLUG/db.sql

/usr/bin/expect <<-EOF
# sudo apt-get install expect

# shell脚本不设置超时时间(默认10s，对应set timeout 10)
set timeout -1

# spawn新建一个进程，这个进程的交互由expect控制
spawn scp root@www.xxx.com:$BACKUP_DIR ./

# expect等待接受进程返回的字符串，直到超时时间，根据规则决定下一步操作
# 模式-动作(多分支模式)
expect {
    "(yes/no)?" {
    	# 发送字符串给expect控制的进程
        send "yes\n"
		# 模式-动作(单分支模式)
        expect "*password*" {send "123456\n"}
    }
    "*password*" {
        send "123456\n"
    }
}

expect "100%"
# 等待spawn进程结束后退出信号eof(通常与interact二选一作为结束)
expect eof
# 将脚本的控制权交给用户，用户可继续输入命令(通常与expect eof二选一作为结束)
# interact
EOF

# -e的作用是使带反斜杠'\'的转义符生效,但是奇怪的是在脚本这里不加-e才是正确的
# echo -e '\n\n'
echo '\n\n'
echo $(date +'%Y%m%d %H:%M:%S %Z') DB size: $(du -h ./db.tar.gz)

echo '\n\n'
tar -zxvf db.tar.gz
mv $UNZIP_FILE ./
rm -rf ./mnt

# 这个脚本只能在项目的虚拟环境中执行
python manage.py reset_db --noinput
mysql -hlocalhost -uroot -pcyagenB15 cyagen_cn < db.sql
mv db.sql db_$SLUG.sql
rm -f db.tar.gz
```



分析：

1. `<<-EOF`获取stdin,并在EOF处结束stdin，输出stdout

   > 值得一提的是`<<-EOF`和`<<EOF`的区别，`<<-EOF`输入行的开头部分的制表符(Tab)都将被去除, 而`<<EOF`不会。这可以解决由于脚本中的自然缩进产生的制表符。

2. 对于`#!/bin/expect`的为第一行的脚本，表示脚本的解释程序位置在`/bin/expect`, 如此不能使用 `sh script.sh` 的方式(该方式的只能启动第一行为 `#!/usr/bin/sh`的脚本)， 需要先赋予脚本执行权限 ---- `chmod a+x  script.sh`。详见：[expect spawn not found](https://blog.csdn.net/chinabluexfw/article/details/7461944)https://blog.csdn.net/chinabluexfw/article/details/7461944)

3. expect eof 和interact 二者可以根据情况选一个作为结尾，一般我们使用 expect eof 。

   expect eof 表示交互结束，退回到原用户；

   interact 会停留在目标用户。

   https://blog.csdn.net/modi000/article/details/107115286/

4. `echo` 在终端输出字符带有转义字符的时候，需要转义符生效需要加 `-e` 参数。朋友电脑加 -e 正常，但是我这里加 `-e` 不正常，如下图（图一是朋友电脑的情况，图二是我的电脑，这是个问题，待研究）。

![1606274603245](http://blog.cdn.ionluo.cn/blog/1606274603245.png)

![1606274625937](http://blog.cdn.ionluo.cn/blog/1606274625937.png)

> 2020-01-07 更新： 关于-e的问题，这里给出了解答。https://blog.csdn.net/liudsl/article/details/79213390
>
> 原因是：GNU/Linux操作系统中的/bin/sh本是bash (Bourne-Again Shell) 的符号链接，但鉴于bash过于复杂，有人把bash从NetBSD移植到Linux并更名为dash (Debian Almquist Shell)，并建议将/bin/sh指向它，以获得更快的脚本执行速度。Dash Shell 比Bash Shell小的多，符合POSIX标准。
>
> Ubuntu继承了Debian，所以从Ubuntu 6.10开始默认是Dash Shell。





## 我的脚本（通过expect登录远程主机执行脚本部署）

```bash
#!/bin/bash

set -e

# 这里用bash是因为jenkins只能执行shell脚本

host='www.xxx.com'
project_dir='/var/www/xxx/'

user='root'
password='123456'


/usr/bin/expect <<-EOF
    set timeout 120

    # 用阿里云服务器当跳板机解决SSH无法连接的问题
    spawn ssh root@www.xxx.com
    expect {
        "(yes/no)?" {send "yes\r";exp_continue}
        "*password*" {send "$password\r"}
    }
    expect "root@*"
	
    if { "$host" != "www.xxx.com" } {
      send "ssh $user@$host\r "
      expect {
          "(yes/no)?" {send "yes\r";exp_continue}
          "*password*" {send "$password\r";exp_continue}
          # 如果是本机是无法跳转的, 如果不加expect的判断可以用这个，就不用判断
          # "root@*" {send "\r"}
      }
    }
    expect "root@*"
    send "cd ${project_dir}\r "
    expect "root@*"
    send "/bin/bash deploy.sh \r "
    expect "root@*"
    expect eof
EOF
```





参考：

https://www.cnblogs.com/su-root/p/11229316.html

https://blog.csdn.net/jacky0922/article/details/45071817

https://www.cnblogs.com/TDXYBS/p/11012089.html