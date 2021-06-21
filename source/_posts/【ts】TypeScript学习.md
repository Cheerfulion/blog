---
title: 【ts】TypeScript学习
date: 2021-06-14 10:06:32
tags:
	- 前端
	- typescript
---



## 文档

https://www.tslang.cn/index.html



## 教程

https://www.bilibili.com/video/BV1Jv41177bD



## 项目开始

```bash
# 创建项目文件夹
mkdir typescript_learning
# 安装yarn这个包管理工具
npm install -g yarn
# 使用yarn安装typescript
yarn add typescript -D
# 安装ts调试工具（https://www.npmjs.com/package/ts-node-dev）
yarn add ts-node-dev --dev
# 使用命令运行src目录下的demo1.ts程序（类似node调试工具nodemon，可以监听文件的变化）
ts-node-dev --respawn --transpile-only src/demo1.ts
# 由于命令较长，推荐写到package.json的scipts中去，这样子只需要运行npm run xxx
```

> 项目代码位置：https://gitee.com/cheerfulion/my_public_demos/tree/master/typescript_learning



## Typescript基础数据类型

![image-20210621104345088](http://blog.cdn.ionluo.cn/blog/image-20210621104345088.png)