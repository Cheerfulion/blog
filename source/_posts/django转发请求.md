---
title: django转发请求
date: 2021-04-07 16:19:15
tags: web开发 后台 django
---



这里使用django转发请求并且添加CORS请求头的方式来绕过图片的跨域限制。

库：[django-proxy==1.2.1](https://github.com/mjumbewu/django-proxy)

```python
# urls.py
from views import my_proxy_view

# views.py
from django.views.decorators.csrf import csrf_exempt
from proxy.views import proxy_view

@csrf_exempt
def my_proxy_view(request, path):
    extra_requests_args = {}
    remoteurl = path
    response = proxy_view(request, remoteurl, extra_requests_args)
    response.__setitem__('Access-Control-Allow-Origin', '*')
    return response
```

