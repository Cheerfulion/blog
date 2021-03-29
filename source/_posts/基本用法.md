---
title: 基本用法
description: '-'
tags:
  - IDE
  - Typora
abbrlink: 8c2ae192
date: 2021-03-29 23:21:50
---



## 下载最新版本Typora

![image-20210316225218741](http://blog.cdn.ionluo.cn/blog/image-20210316225218741.png)





##  设置图片上传位置

> 文件 → 偏好设置 → 图像

```json
// 配置文件 -- 这里以我的七牛云图床为例，其他自行百度
{
  "picBed": {
    "uploader": "qiniu",
    "qiniu":{
      "accessKey": "your accessKey",
      "secretKey": "your secretKey",
      "bucket": "your bucket name",   // 空间名
      "url": "http://blog.cdn.ionluo.cn",   // 空间域名
      "area": "z2",  // 存储区域（华东z0, 华北z1, 华南z2, 北美na0， 东南亚as0）
      "options": "" ,// 网址后缀，比如?imgslim
      "path": "blog/"  // 自定义存储路径
    }
  },
  "picgoPlugins": {}
}
```



![image-20210316225539242](http://blog.cdn.ionluo.cn/blog/image-20210316225539242.png)



如果验证图片上传选项成功，则就没有什么问题了，可以和我一样设置插入本地图片时上传，这样子本地图片就会上传到七牛云，同时你的本地也会保存这张图片（保存位置可以断开网络来插入图片查看）。



## 遇到的坑

1. 批量上传图片顺序混乱

   > **批量上传图片：**格式 → 图像 → 上传所有本地图片

   **【重要】使用Typora的同学尽量不要使用到该功能，否则又要排版一次了。**

   解决办法：在七牛云控制台上传文件，上传后用typora的替换功能把图片的前面地址换成七牛云的地址就可以了。也就是说，上传七牛云是不会改变文件名的，但是要注意同名的情况。

2. 没有网络的情况下，图片就无法访问了

   - 通过nginx代理把图片请求转到本地目录。难度一颗星。
   - 开启一个服务，修改host来让七牛云域名指向localhost。该方法对运维知识需要稍有了解，难度三颗星。