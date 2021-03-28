---
title: gitlab部署秘钥并添加到项目中，设置免密验证
description: 暂无描述！
tags:
  - Git
abbrlink: de167b2b
date: 2021-03-16 22:39:09
---



1. 目标服务器上设置秘钥 `ssh-keygen`
   ![20200814155645489](http://blog.cdn.ionluo.cn/blog/20200814155645489.png)

   

   **如上图所示，如果存在秘钥的，直接复制秘钥 `cat /root/.ssh/id_rsa.pub`**

2. 登录gitlab管理员账号，点击左上角设置，选择部署秘钥，再点击创建秘钥
   ![20200814155724685](http://blog.cdn.ionluo.cn/blog/20200814155724685.png)
   ![20200814155817149](http://blog.cdn.ionluo.cn/blog/20200814155817149.png)

3. 将id_rsa.pub的内容添加到key，标题用于标识可以随便写，然后点击左下角创建就行了。
   ![202008141559151](http://blog.cdn.ionluo.cn/blog/202008141559151.png)

4. 点击左上角项目，选择你的项目，再点击设置，选择存储库，再选择部署秘钥

   ![20200814160023893](http://blog.cdn.ionluo.cn/blog/20200814160023893.png)

5. 点进去后往下拉，找到刚刚添加的秘钥，再点击启用就行了。

   ![20200814160135457](http://blog.cdn.ionluo.cn/blog/20200814160135457.png)