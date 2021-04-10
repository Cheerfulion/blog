---
title: 你用对 hasattr 了嘛？
description: '-'
tags:
  - WEB开发
  - Web后端
  - python
abbrlink: 18af7293
date: 2021-02-23 21:51:33
---



## 前言

今天市场同学反馈了一个问题，网站的默认SEO信息不正确，显示了默认的SEO标题。一番排查，发现所有问题都指向了 `hasattr`

```python
from django.conf import settings

@property
def seo(self):
    if hasattr(self, 'suggest_seo'):
        result = self.suggest_seo
    else:
        result = settings.DEFAULT_SEO
```



上面的 `hasattr(self, 'suggest_seo'):` 居然是`False`, 自己写了个Demo又是正确的。因此借助搜索引擎，找到了下面文章解决了我的疑惑：[你用对 hasattr 了嘛？](https://www.dongwm.com/post/becareful-hasattr/)。 



简而言之，就是说属性在计算的时候报错就会导致`hasattr`为`False`。 也就是说在 Python2，如果你想要判断一个类里面是否有某个 property，需要确认它会不会有机会抛错。如果它有可能会异常，那么应该用 getattr（让异常发生）。



下面摘录一下原文：





## 你用对 hasattr 了嘛？

前天帮一个同学 DEBUG 一个很奇怪的问题，发现了一个 Python 2 的 hasattr 一个不适用场景，和大家分享一下。

在日常开发中，判断对象有没有一个属性，你可能这么写：

```python
if hasattr(self, 'redirect_url'):
    return request.redirect(self.redirect_url)
```

如果 self 有 redirect_url 这个属性，那么请求直接重定向到 redirect_url 这个 URL 上。看起来没有问题对吧，我们写的更完整一些：

```python
In [1]: class VideoSubject(object):
   ...:     @property
   ...:     def redirect_url(self):
   ...:         raise IndexError()  # 就是让它抛错
   ...:

In [2]:

In [2]: vs = VideoSubject()

In [3]: hasattr(vs, 'redirect_url**)
Out[3**: False
```

看到了吧 hassattr 的结果是 False，也就是说 Python 2 认为 vs 这个实例就没有 redirect_url 属性。但是看代码是有这个 propery 的，只是在计算时由于某些原因抛错误了。

我 debug 了好久才发现这个原来是这个问题，🤦‍

感觉网上搜了下，原来 attrs 的作者，Python 核心开发在 16 年就写过一篇文章介绍这个事情：

https://hynek.me/articles/hasattr/

也就是说**在 Python2，如果你想要判断一个类里面是否有某个 property，需要确认它会不会有机会抛错**

如果它有可能会异常，那么应该用 getattr（让异常发生）：

```python
In [4]: getattr(vs, 'redirect_url', None)
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-4-08e45e416015> in <module>()
----> 1 getattr(vs, 'redirect_url', None)

/Users/dongweiming/workspace/test.py in redirect_url(self)
      4     @property
      5     def redirect_url(self):
----> 6         raise IndexError()  # 就是让它抛错
      7
      8

IndexError:
```

另外在 CPython 实现，hasattr 其实内部还是用了 getattr 的:

```python
/* 2.7/Python/bltinmodule.c#L828-L858 */
static PyObject *
builtin_getattr(PyObject *self, PyObject *args)
{
    PyObject *v, *result, *dflt = NULL;
    PyObject *name;

    if (!PyArg_UnpackTuple(args, "getattr", 2, 3, &v, &name, &dflt))
        return NULL;
#ifdef Py_USING_UNICODE
    if (PyUnicode_Check(name)) {
        name = _PyUnicode_AsDefaultEncodedString(name, NULL);
        if (name == NULL)
            return NULL;
    }
#endif

    if (!PyString_Check(name)) {
        PyErr_SetString(PyExc_TypeError,
                        "getattr(): attribute name must be string");
        return NULL;
    }
    result = PyObject_GetAttr(v, name);
    if (result == NULL && dflt != NULL &&
        PyErr_ExceptionMatches(PyExc_AttributeError))
    {
        PyErr_Clear();
        Py_INCREF(dflt);
        result = dflt;
    }
    return result;
}
```

注意`result = PyObject_GetAttr(v, name)`这步，所以 getattr 要比 hasattr 更快更直接。

而在 Python 3 没有这个问题，他会捕获 AttributeError 返回 False，而其他错误直接抛出来:

```python
/*
3.7/Objects/object.c#L1231-L1242
阅读路线： builtin_hasattr_impl -> _PyObject_LookupAttr -> PyObject_GenericGetAttr -> _PyObject_GenericGetAttrWithDict
* /
if (descr != NULL) {
    Py_INCREF(descr);
    f = descr->ob_type->tp_descr_get;
    if (f != NULL && PyDescr_IsData(descr)) {
        res = f(descr, obj, (PyObject *)obj->ob_type);
        if (res == NULL && suppress &&
            PyErr_ExceptionMatches(PyExc_AttributeError)) {
            PyErr_Clear();
        }
        goto done;
    }
}
```

也就抛出了 PyExc_AttributeError 会被清除（其他的错误照常 raise）

### 延伸阅读

1. https://hynek.me/articles/hasattr/
2. https://github.com/python/cpython/blob/3.7/Objects/object.c#L1231-L1242