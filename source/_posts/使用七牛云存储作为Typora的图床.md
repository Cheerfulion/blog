---
title: 使用七牛云存储作为Typora的图床
description: '-'
tags:
  - 七牛云
abbrlink: b20a6fa3
date: 2021-03-06 12:57:00
---



## 环境

`Typora for Windows/Linux   version 0.9.98(beta)`

`实名认证后的七牛云账号`

`备案后的个人域名`



## 七牛云

1. 创建存储空间

   > 对象存储 > 空间管理 > 新建空间

   ![image-20210224112826313](http://blog.cdn.ionluo.cn/blog/image-20210224112826313.png)

2. 个人域名绑定

   > 点击新建的存储空间 > 域名管理 > 自定义CDN加速域名 > 绑定域名

   **加速域名前缀可以自定义，后面通过CNAME解析即可创建该域名。如我购买的域名是`ionluo.cn`, 我这里就填写 `blog.cdn.ionluo.cn`**。后面点击创建会有文档提示如何解析。

   ![image-20210224113748580](http://blog.cdn.ionluo.cn/blog/image-20210224113748580.png)

   切换回`空间管理`， 就可以看到绑定的CDN加速域名了。

   ![image-20210224114004307](http://blog.cdn.ionluo.cn/blog/image-20210224114004307.png)

   > 因为我用的都是免费的服务，所以我这里就不开启HTTPS了，如果需要开启HTTPS，产生的流量会计费。如果一定要HTTPS，也可以在七牛云上白嫖一个SSL证书，这个可以看我的另外一篇博客[SSL免费证书申请]()

3. 查看`accessKey/secretKey`

   ![image-20210224114233865](http://blog.cdn.ionluo.cn/blog/image-20210224114233865.png)

   如此，Typora需要的七牛云配置和信息就获取到了。



## Typora

1. 安装图片上传插件

   > 文件 > 偏好设置 > 图像 > 上传服务设定 > 上传服务 > PicGo-Core (command line) > 下载或更新

   ![image-20210224114521637](http://blog.cdn.ionluo.cn/blog/image-20210224114521637.png)

   下载速度会比较慢，需要耐心等待，下载完成后就可以点击“打开配置文件”了

2. 填写配置文件

   ```json
   {
     "picBed": {
       "uploader": "qiniu",
       "qiniu":{
         "accessKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
         "secretKey": "yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy",
         "bucket": "ionluo-blog",   // 空间名
         "url": "http://blog.cdn.ionluo.cn",   // 空间域名
         "area": "z2",  // 存储区域（华东z0、华南z1、华北z2、北美na0、东南亚as0）
         "options": "" ,// 网址后缀，比如?imgslim
         "path": "blog/"  // 自定义存储路径
       }
     },
     "picgoPlugins": {}
   }
   ```

3. 测试

   保存配置文件后，点击上传服务下面的`验证图片上传选项`按钮，如下即成功了。在七牛云存储空间的文件管理里面可以看到上传的两张图片。

   ![image-20210224114940318](http://blog.cdn.ionluo.cn/blog/image-20210224114940318.png)





## 问题

1. 七牛云的文件默认是不允许跨域请求的，所以如果要复制博客到其他地方，如CSDN上，可以如下配置。

   > 对象存储 > 空间管理 > 域名管理 > blog.cdn.ionluo.cn > HTTP 响应头配置 > 保存 > 等待生效
   >
   > ![image-20210224115358618](http://blog.cdn.ionluo.cn/blog/image-20210224115358618.png)

2. 如果自己本身有一个博客站点，静态文件保存在博客站点里面，希望自动同步到七牛云存储上，或者使用CDN加速自己的博客站点，不用每次更新都手动上传文件到七牛云存储，可以在 `空间管理 > 空间设置 > 镜像回源` 里面设置自己站点的回源地址，这样子七牛云存储不存在的文件就会自己去源站同步，实现数据平滑迁移了。

   ![image-20210225104214601](http://blog.cdn.ionluo.cn/blog/image-20210225104214601.png)

