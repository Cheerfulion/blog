---
title: 【转载】 解决jenkins点击立即构建没有反应
description: '-'
tags:
  - Jenkins
abbrlink: 227dc26b
date: 2021-03-28 18:05:48
---



### 一、问题

在Jenkins上点击`立即构建`没有反应

### 二、锁定问题导致根本原因

###### 1、查看jenkins服务进程号

> 温馨小提示：这里基于`java -jar jenkins.war`方式运行的jenkins

```shell
jps -l
```

![在这里插入图片描述](http://blog.cdn.ionluo.cn/blog/20201225103417250.png)

###### 2、查看jenkins进程信息

```shell
ps -aux |grep 进程号
```

> 这里主要看下之前启动时指定的日志文件，小编这里是默认jenkins.war所在路径的默认输出文件`nohup.out`

![在这里插入图片描述](http://blog.cdn.ionluo.cn/blog/20201225104406737.png)

###### 3、查看jenkins日志文件错误信息

```shell
# 查看jenkins安装目录
ls -l /proc/进程号/cwd
```

![在这里插入图片描述](http://blog.cdn.ionluo.cn/blog/20201225103536976.png)

```shell
cd /home/rhpassadmin/jenkins
ls
```

![在这里插入图片描述](http://blog.cdn.ionluo.cn/blog/20201225104754512.png)
查看日志文件后100行信息

```shell
tail -n 100 nohup.out
```

可以看到`java.io.IOException: No space left on device`， 提示磁盘空间不足
![在这里插入图片描述](http://blog.cdn.ionluo.cn/blog/20201225105333655.png)

###### 4、查看磁盘空间

```shell
df -h
```

发现jenkins所在路径的已用空间`100%`

![在这里插入图片描述](http://blog.cdn.ionluo.cn/blog/20201225103912236.png)

### 三、解决

到此问题发生原因找到也已经确认是`磁盘空间不足`所导致无法正常构建，那么赶紧去`释放空间`吧！！！
