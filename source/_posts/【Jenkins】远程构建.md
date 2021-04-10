---
title: 远程构建
description: '-'
tags:
  - Jenkins
abbrlink: c0fa8060
date: 2021-02-12 13:31:14
---



## 通过expect登录远程主机执行脚本部署

```shell
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



参考(一些编写脚本的时候看的文章)：

https://www.cnblogs.com/TDXYBS/p/11012089.html

http://xstarcd.github.io/wiki/shell/expect.html

https://www.cnblogs.com/su-root/p/11229316.html

https://blog.csdn.net/jacky0922/article/details/45071817



> 更多关于expect的可以看 运维 -> Linux -> 通过expect自动化地执行命令行交互任务 

