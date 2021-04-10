---
title: nginx多个斜杆依旧可以访问问题
description: '-'
tags:
  - Nginx
abbrlink: ea0c396f
date: 2021-03-31 20:15:42
---



## 问题复现

我的nginx配置

```json
server {
    listen  6001;
    server_name localhost;

    add_header Access-Control-Allow-Origin * always;
    add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS' always;
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization' always;
    if ($request_method = 'OPTIONS') {
        return 204;
    }

location / {
    root C:\Users\Administrator\Documents\HBuilderProjects\blank;
    index  new_file_3.html;
    autoindex	on;
}
}
```

启动nginx后，浏览器访问 `http://localhost:6001/////`居然可以正常访问，如图：

![image-20210331140737160](http://blog.cdn.ionluo.cn/blog/image-20210331140737160.png)





## 解决方法

暂无