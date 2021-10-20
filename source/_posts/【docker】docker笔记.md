---
title: 【docker】docker笔记
tags:
  - docker
abbrlink: c46cd1c6
date: 2021-08-09 14:45:14
---





## 前言

记录一些docker的一些使用方案和使用过程遇到的问题和解决方法



## 入门

http://guide.daocloud.io/dcs/docker-9152673.html

https://www.ionluo.cn/blog/posts/c402862b.html



## [使用 docker 构建前端统一开发环境](https://blog.csdn.net/weixin_43365116/article/details/114241529)

### 开发新项目常见情形

- git clone 下载项目源码
- npm install 下载项目依赖
- npm run build 构建项目

**存在的问题：**

- 操作系统版本问题，如macos win 和 linux 文件名大小写问题
- 运行环境差异，node-sass版本和node版本问题
- 安装的第三方包版本不匹配报错
- 开发环境和测试环境割裂

**想要达到的效果**

- 在不同的开发机上表现一致，与开发机系统无关
- 本地安装的基础环境(主要是node版本)不影响项目运行
- 非前端环境一键配置
- 其余人接手项目后，可以快速搭建好环境并运行项目



### 两个基本概念

镜像(image)：一套极简的 os + 基础运行环境

容器(container)：运行的镜像实例



### docker 简单命令

**拉取镜像**

```bash
docker pull docker/getting-started
```

**运行容器**

```bash
docker run -d -p 80:80 docker/getting-started
```

**查看运行中的容器**

```bash
docker ps
```

**进入容器**

```bash
docker exec -it <the-container-id> /bin/sh
```

**停止运行容器**

```bash
docker stop <the-container-id>
```

**删除非运行状态下的容器**

```bash
docker rm <the-container-id>
```

**停止并删除容器**

```bash
docker rm -f <the-container-id>
```



### 使用 Dockerfile 构建镜像

**Dockerfile.api**

```bash
FROM node:12-alpine
RUN apk add git
WORKDIR /app
RUN git clone https://github.com/Binaryify/NeteaseCloudMusicApi.git
WORKDIR /app/NeteaseCloudMusicApi
RUN npm config set registry https://registry.npm.taobao.org 
RUN npm install
CMD [ "node", "app.js" ]
```

**Dockerfile.app**

```bash
FROM node:12-alpine
RUN mkdir /app
WORKDIR /app
RUN npm config set registry https://registry.npm.taobao.org 
COPY package*.json ./
RUN npm install
COPY . .
CMD npm run dev
```

**构建镜像**

```bash
docker build -t NeteaseCloudMusicApi .
```

**运行 NeteaseCloudMusicApi 镜像**

```bash
docker run -dp 3000:3000 NeteaseCloudMusicApi
```

至此，本地已启动了服务



### 使用 docker-compose 启动

```bash
version: "3.7"

services: 
  api:
    build: 
      context: .
      dockerfile: Dockerfile.api
    container_name: music_api
    ports: 
      - "3000:3000"
  
  app:
    build: 
      context: .
      dockerfile: Dockerfile.app
    container_name: music_app
    links: 
      - api:api
    working_dir: /app
    volumes: 
      - './:/app'
      - '/app/node_modules'
    ports: 
      - "9999:9999"
    command: npm run dev
```

**启动项目**

```bash
docker-compose up
```



## win10安装docker

https://blog.csdn.net/zzq060143/article/details/91050272



## docker 搭建node+vue应用

1. 使用nginx+vue打包好静态页面
2. 使用node+vue直接跑起来



```bash
# 先查看当前node的版本（v14.0.0）
node -v

# 到dockerhub中下载指定版本node镜像
# https://hub.docker.com/_/node?tab=tags&page=1&ordering=last_updated&name=14.0.0
docker pull node:14.0.0-alpine


```





## 遇到的问题

