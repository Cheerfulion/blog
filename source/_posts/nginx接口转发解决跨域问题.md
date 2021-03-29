---
title: nginx接口转发解决跨域问题
description: '-'
tags:
  - Nginx
abbrlink: 52ec706e
date: 2021-01-04 22:17:45
---







```bash
server {
    listen  2077;  # 开启端口
    server_name 172.20.0.166; # 本机局域网ip

    location / {
        if ($request_method = 'OPTIONS') {
            add_header Access-Control-Allow-Origin * always;
            add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS' always;
            add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization' always;
            return 204;
        }

        rewrite ^/(.*)$ /$1 break;
        proxy_pass https://bio.cyagen.net;
    }
}
```

