---
title: Ubuntu升级node和npm
description: '-'
tags:
  - WEB开发
  - Web后端
  - NodeJs
abbrlink: 455e7f2f
date: 2021-03-31 21:21:08
---



> 16和18版本的最简单安装方式不同，因此分开介绍.

## Ubuntu16.04安装最新版nodejs

##### 更新ubuntu软件源

```csharp
sudo apt-get update
sudo apt-get install -y python-software-properties software-properties-common
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
```

##### 安装nodejs

```csharp
sudo apt-get install nodejs
sudo apt install nodejs-legacy
sudo apt install npm
```

##### 更新npm的包镜像源，方便快速下载

```cpp
sudo npm config set registry https://registry.npm.taobao.org
sudo npm config list
```

##### 全局安装n管理器(用于管理nodejs版本)

```undefined
sudo npm install n -g
```

##### 安装最新的nodejs（stable版本）

```undefined
sudo n stable
sudo node -v
```



## Ubuntu18.04安装最新版nodejs

1、下载最新的稳定版本nodejs: [https://nodejs.org/zh-cn/down...](https://nodejs.org/zh-cn/download/)
![image.png](http://blog.cdn.ionluo.cn/blog/bVbDYjM)

```bash
# 下载最新稳定版本的nodejs
wget https://nodejs.org/dist/v12.16.1/node-v12.16.1-linux-x64.tar.xz
```

2、在本地解压，提取文件，把解压文件移动到/usr/local/目录下（需要root权限）

```bash
# 本地解压
tar -xvf node-v12.16.1-linux-x64.tar.xz

# 将解压后的文件夹整体移动到/usr/local/node
sudo mv node-v12.16.1-linux-x64 /usr/local/node
```

3、在/usr/bin 目录下建立软连接

```bash
# 切换目录
cd /usr/bin
# 创建node软链接
sudo ln -s /usr/local/node/bin/node node
# 创建npm软链接
sudo ln -s /usr/local/node/bin/npm npm
# 设置淘宝镜像源
sudo npm config set registry https://registry.npm.taobao.org
```

4、查看安装

```bash
node -v
npm -v
```

![image.png](https://segmentfault.com/img/bVbDYoh)





## 问题记录

1. 升级nodejs后如果运行npm install或者其他命令依旧提示node或者npm版本过低，新开一个终端即可。因为新开终端才会重新读取环境。



## 参考

1. [[Ubuntu18安装nodejs](https://segmentfault.com/a/1190000021880964)](https://segmentfault.com/a/1190000021880964)

2. [Ubuntu16.04安装最新版nodejs](https://www.jianshu.com/p/2b24cd430a7d)