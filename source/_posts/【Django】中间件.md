---
title: 中间件
description: '-'
tags:
  - WEB开发
  - Web后端
  - Django
abbrlink: e919cac4
date: 2021-03-29 23:12:41
---





## 前言

最近需要实现一个让Django应用template缺失的时候返回404而不是500，发现存在该问题的页面很多，于是想到通过中间件实现，代码如下：

```python
from django.template import TemplateDoesNotExist
from django.views.defaults import page_not_found

class TemplateDoesNotExistMiddleware(object):
    """ 
    If this is enabled, the middleware will catch
    TemplateDoesNotExist exceptions, and return a 404
    response.
    """

    def process_exception(self, request, exception):
        if isinstance(exception, TemplateDoesNotExist):
            return page_not_found(request)
```

本以为大功告成，测试发现并没有左右，中间件未触发。因此发现自己对中间件了解不够细致，于是根据[官方文档](https://docs.djangoproject.com/en/1.8/topics/http/middleware/)做了下笔记。也可以直接参考官方文档：https://docs.djangoproject.com/en/1.8/topics/http/middleware/

> 该文章的Django版本是1.8.4



## 正文

### 介绍

Django 中间件是修改 Django request 或者 response 对象的钩子，可以理解为是介于 HttpRequest 与 HttpResponse 处理之间的一道处理过程。

浏览器从请求到响应的过程中，Django 需要通过很多中间件来处理，可以看如下图所示：

![img](http://blog.cdn.ionluo.cn/blog/1_t9TAX89Y3rZUXth2Le07Xg.png)





在请求阶段，Django会在调用视图之前按[`MIDDLEWARE_CLASSES`](https://docs.djangoproject.com/en/1.8/ref/settings/#std:setting-MIDDLEWARE_CLASSES)从上至下定义的顺序应用中间件。有两个钩子可用：

- [`process_request()`](https://docs.djangoproject.com/en/1.8/topics/http/middleware/#process_request)
- [`process_view()`](https://docs.djangoproject.com/en/1.8/topics/http/middleware/#process_view)

在响应阶段，调用视图后，从下至上以相反的顺序应用中间件。提供三个挂钩：

- [`process_exception()`](https://docs.djangoproject.com/en/1.8/topics/http/middleware/#process_exception) （仅当视图引发异常时）
- [`process_template_response()`](https://docs.djangoproject.com/en/1.8/topics/http/middleware/#process_template_response) （仅适用于模板响应）
- [`process_response()`](https://docs.djangoproject.com/en/1.8/topics/http/middleware/#process_response)





要激活中间件组件，请将其添加到[`MIDDLEWARE_CLASSES`](https://docs.djangoproject.com/en/1.8/ref/settings/#std:setting-MIDDLEWARE_CLASSES)Django设置的 元组中。

[`MIDDLEWARE_CLASSES`](https://docs.djangoproject.com/en/1.8/ref/settings/#std:setting-MIDDLEWARE_CLASSES)的顺序很重要，因为中间件可以依赖于其他中间件。

Django 默认的中间件配置：

```python
MIDDLEWARE_CLASSES = (
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
)
```



### 编写您自己的中间件[¶](https://docs.djangoproject.com/en/1.8/topics/http/middleware/#writing-your-own-middleware)

编写自己的中间件很容易。每个中间件组件都是单个Python类，该类定义以下一个或多个方法：

```python
def process_request(self, request)
def process_view(self, request, view_func, view_args, view_kwargs)
def process_exception(self, request, exception)
def process_template_response(self, request, response)
def process_response(self, request, response)
```



> 上面的是按从上往下的顺序执行的，但是process_exception和process_template_response是在特定情况下才会执行的。前2个是顺序执行，后3个是倒序执行。有多个中间件时，会按照定义的元组顺序，依次执行每个中间件的process_request，然后是每个中间件的process_view，依次类推。





### SHOW CODE

```python
# setting.py
MIDDLEWARE_CLASSES = (
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    
    "app.middleware.Test1Middleware",
    "app.middleware.Test2Middleware",
)
```





```python
# project/app/middleware.py
class Test1Middleware(object):

    # 在视图函数之前执行
    def process_request(self, request):
        print("process_request_1", id(request))
        # 返回值是 None 的话，按正常流程继续走。(默认就是返回None)
        # return None
        # 返回值是 HttpResponse 对象，Django 将process_response中间件应用于HttpResponse
        # return HttpResponse("fail")

    # 在视图函数之前，process_request 方法之后执行
    # view_func 是 Django 即将使用的视图函数。 view_args 是将传递给视图的位置参数的列表。view_kwargs 是将传递给视图的关键字参数的字典。
    def process_view(self, request, view_func, view_args, view_kwargs):
        print("process_view_1", id(request))
        # 返回值是 None 的话，按正常流程继续走。(默认就是返回None)
        # return None
        # 返回值是 HttpResponse 对象，Django 将process_response中间件应用于HttpResponse
        # return HttpResponse("fail")

    # 在视图函数之后，在 process_response 方法之前执行
    # 只有在视图函数中出现异常了才执行。exception 是视图函数异常产生的 Exception 对象。
    def process_exception(self, request, exception):
        print("process_exception_1", id(request))
        # 返回值是 None 的话，启动默认异常处理。
        # return None
        # 返回值是 HttpResponse 对象，则将执行process_template_response和process_response中间件，并将生成的响应返回给浏览器
        # return HttpResponse("fail")

    # 在视图函数之后，在 process_response 方法之前执行
    # 只有视图响应实例是一个模板响应，如render()方法　才会执行
    def process_template_response(self, request, response):
        print("process_template_response_1", id(request))
        # 它必须返回实现render方法的响应对象。它可以response通过更改response.template_name和response.context_data来更改模板响应，也可以创建并返回一个全新的模板响应。
        return response

    # 在视图函数之后执行的
    # 不同于其他方法，该方法必定会调用。
    def process_response(self, request, response):
        print("process_response_1", id(request))
        # 该方法必须要有返回值且必须是response
        return response


class Test2Middleware(object):

    def process_request(self, request):
        print("process_request_2", id(request))

    def process_view(self, request, view_func, view_args, view_kwargs):
        print("process_view_2", id(request))
        # return view_func(request)

    def process_exception(self, request, exception):
        print("process_exception_2", id(request))

    def process_template_response(self, request, response):
        print("process_template_response_2", id(request))
        return response

    def process_response(self, request, response):
        print("process_response_2", id(request))
        return response
```



```python
# view.py
from django.views.generic import ListView, DetailView, TemplateView

class WebTemplateView(TemplateView):
    def __init__(self):
        raise ValueError("这是一个错误")
        self.template_name = 'xxx.html'
        super(WebTemplateView, self).__init__()
        
    def get(self, request, *args, **kwargs):
        return super(WebTemplateView, self).get(request, *args, **kwargs)
    
    def get_context_data(self, **kwargs):
        context = super(WebTemplateView, self).get_context_data(**kwargs)
        return context
```



结果：

```bash
# 视图存在报错
('process_request_1', 140159067726160)
('process_request_2', 140159067726160)
('process_view_1', 140159067726160)
('process_view_2', 140159067726160)
('process_exception_2', 140159067726160)
('process_exception_1', 140159067726160)
('process_response_2', 140159067726160)
('process_response_1', 140159067726160)


# 去掉视图中的错误，则结果是：
('process_request_1', 140277798695056)
('process_request_2', 140277798695056)
('process_view_1', 140277798695056)
('process_view_2', 140277798695056)
('process_template_response_2', 140277798695056)
('process_template_response_1', 140277798695056)
('process_response_2', 140277798695056)
('process_response_1', 140277798695056)
```



> 注意，上面的代码是随性写的，并不确定能否跑起来，但是思想是这么回事。因为我的项目中代码逻辑更复杂一些，懒得重开一个项目写了。





## 总结

回到最初的问题，这里视图中模板发生了`TemplateDoesNotExist`错误，按道理应该会进入`process_exception`中间件，但是测试并没有进入`process_exception`，但是`process_template_response`是进入了。在`process_template_response`打印response的状态码，发现居然是200，而process_response是500。

```python
def process_template_response(self, request, response):
	print response.status_code
	return response
```

然而这里无法拿到错误对象，`response.content`也是空，陷入死局了。。。



另外这里尝试在视图函数里面捕获这个错误，也无法捕获到。。。

```python
# view.py
from django.views.generic import ListView, DetailView, TemplateView
from django.http.response import Http404
from django.template import TemplateDoesNotExist


class WebTemplateView(TemplateView):
    def __init__(self):
        self.template_name = 'xxx.html'
        super(WebTemplateView, self).__init__()
        
    def get(self, request, *args, **kwargs):
        try:
        	return super(WebTemplateView, self).get(request, *args, **kwargs)
        except:
            # 这里可以更精准匹配下错误类型 except TemplateDoesNotExist， 这里只是确定无法捕获到任何错误
            raise Http404('Page does not exist')
    
    def get_context_data(self, **kwargs):
        context = super(WebTemplateView, self).get_context_data(**kwargs)
        return context
```





最终解决方法，通过 `os.path.isfile()`判断模板文件是否存在：（不是什么好方法，但是可行）

```python
# view.py
from django.views.generic import ListView, DetailView, TemplateView
from django.http.response import Http404

class WebTemplateView(TemplateView):
    def __init__(self):
        # self.template_name = 'xxx.html'
        super(WebTemplateView, self).__init__()
        
    def get_template_names(self, *args, **kwargs):
        template_name = kwargs.get('template_name') or self.template_name
        result = []
        if os.path.isfile(template_name):
            result.insert(0, template_name)
        else:
            raise Http404('Page does not exist')
        return result
        
    def get(self, request, *args, **kwargs):
        return super(WebTemplateView, self).get(request, *args, **kwargs)
    
    def get_context_data(self, **kwargs):
        context = super(WebTemplateView, self).get_context_data(**kwargs)
        return context
    
    
# 其他地方用WebTemplateView代替TemplateView
```

