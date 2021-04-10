---
title: Ubuntu监控内存利用率并邮件告警
description: '-'
tags:
  - 运维
  - Linux
abbrlink: a3db674
date: 2021-02-12 13:31:15
---



邮件告警需要安装`mailutils`包



**方法-1：用 Linux Bash 脚本监视内存利用率并发送电子邮件**

```bash
*/5 * * * * /usr/bin/free | awk '/Mem/{printf("RAM Usage: %.2f%\n"), $3/$2*100}' | awk '{print $3}' | awk '{ if($1 > 80) print $0;}' | mail -s "High Memory Alert" ionluo@xxx.com
```

> 内存占用超过80%发送邮件，邮件内容类似：High Memory Alert: 80.40%

**方法-2：用 Linux Bash 脚本监视内存利用率并发送电子邮件**

可以获取到更多信息。

```bash
# vim /opt/scripts/memory-alert.sh
 
#!/bin/sh
ramusage=$(free | awk '/Mem/{printf("RAM Usage: %.2f\n"), $3/$2*100}'| awk '{print $3}')
 
if [ "$ramusage" > 80 ]; then
 
 SUBJECT="ATTENTION: Memory Utilization is High on $(hostname) at $(date)"
 MESSAGE="/tmp/Mail.out"
 TO="ionluo@xxx.com"
 echo "Memory Current Usage is: $ramusage%" >> $MESSAGE
 echo "" >> $MESSAGE
 echo "------------------------------------------------------------------" >> $MESSAGE
 echo "Top Memory Consuming Process Using top command" >> $MESSAGE
 echo "------------------------------------------------------------------" >> $MESSAGE
 echo "$(top -b -o +%MEM | head -n 20)" >> $MESSAGE
 echo "" >> $MESSAGE
 echo "------------------------------------------------------------------" >> $MESSAGE
 #echo "Top Memory Consuming Process Using ps command" >> $MESSAGE
 #echo "------------------------------------------------------------------" >> $MESSAGE
 #echo "$(ps -eo pid,ppid,%mem,%Memory,cmd --sort=-%mem | head)" >> $MESSAGE
 mail -s "$SUBJECT" "$TO" < $MESSAGE
 rm /tmp/Mail.out
fi
```

给脚本赋予执行权限

```bash
chmod u+x /opt/scripts/memory-alert.sh
```



最后添加一个 [cron 任务 ](https://www.2daygeek.com/crontab-cronjob-to-schedule-jobs-in-linux/)来自动执行此操作。它将每 5 分钟运行一次。

```bash
# crontab -e
*/5 * * * * /bin/bash /opt/scripts/memory-alert.sh
```

设置好后一般要2分钟后会自动生效。

