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

到此问题发生原因找到也已经确认是`磁盘空间不足`所导致无法正常构建，那么赶紧去`释放空间`吧，奥利给！！！

------

> 今日分享语句：
> 太多的人，把努力当成了一种“人设”。
> 想做一件事情，还没动工，就敲锣打鼓；想达成一个目标，八字还没一撇，就高谈阔论。好像他们的努力，不是为了追求结果，而是为了把努力的形式公布于众，像完成某种仪式。
> 如果喊着要努力的人，都可以扎扎实实下功夫，可能这世上的遗憾也会少很多。
> 很多时候，我们与牛人的差距，就是差那么一点脚踏实地的真努力。
> 真正的努力，是“富贵本无根，尽从勤里得”的踏实；是“读书破万卷，下笔如有神”的勤奋；是“欲穷千里目，更上一层楼”的精进。
> 真正的努力，都不喧嚣。只需要卷起袖子，行动起来。