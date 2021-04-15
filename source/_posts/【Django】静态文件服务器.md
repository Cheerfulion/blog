---
title: 静态文件服务器
description: '-'
tags:
  - WEB开发
  - Web后端
  - Django
abbrlink: d2e06aad
date: 2021-01-20 00:15:23
---



## Django设置 DEBUG = False后静态文件无法加载解决

django版本：1.8.4

问题出现的原因：

```
当我们在开发django应用时如果设置了 DEBUG = True，那么django便会自动帮我们对静态文件进行路由；但是当我们设置DEBUG = False后，这一功能便没有了，此时静态文件就会出现加载失败的情况，想要让静态文件正常显示，我们就需要配置静态文件服务了。
```

配置静态文件服务一般有两种情况，**一种是使用文件服务器如nginx来实现：**

```nginx
location /static {
    expires 30d;
    autoindex on;
    add_header Cache-Control private;
    alias /root/myapp/static;
}
```

**第二种就是设置django的文件服务的django.views.static.serve()方法，只需要在urls.py中添加如下内容**

```python
# urls.py

urlpatterns = [
   url(r'^admin/', admin.site.urls),
   ...
] 

urlpatterns += patterns(
    '',
    url(r'^static/(?P<path>.*)$', 'django.views.static.serve', {'document_root': settings.STATIC_ROOT}),
)
```

```python
# setting.py

import os

PROJECT_PATH = os.path.normpath(os.path.dirname(os.path.abspath(__file__)))

STATIC_URL = '/static/'

STATIC_ROOT = os.path.join(PROJECT_PATH, 'app/static')
```

> 该方法不好自定义一些功能，如我就发现图片不存在的时候请求时间很长，卡页面。另外就是无法设置跨域请求头Access-Control-Origin: *; 所以建议自己重写一个视图函数，不要直接使用django.views.static.serve