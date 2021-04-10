---
title: django中文件下载
description: '-'
tags:
  - WEB开发
  - Web后端
  - Django
abbrlink: ced0c9f4
date: 2021-04-01 23:31:40
---



## 我的Word文件下载类

有一些还没有封装好，我这里只提供了文件下载的方法。我这里的需求是html转word，希望弄成可以html预览和word文件下载两种自由切换的形式。

可以参考[django-wkhtmltopdf](https://pypi.org/project/django-wkhtmltopdf/)库的实现方式。

```python
from django.http import HttpResponse

class WordResponse(HttpResponse):
    """HttpResponse that sets the headers for WordResponse output."""

    def __init__(self, content, status=200, content_type=None,
            filename=None, show_content_in_browser=None, *args, **kwargs):

        if content_type is None:
            content_type = 'application/msword'

        super(WordResponse, self).__init__(content=content,
                                          status=status,
                                          content_type=content_type)
        self.set_filename(filename, show_content_in_browser)

    def set_filename(self, filename, show_content_in_browser):
        self.filename = filename
        if filename:
            fileheader = 'attachment; filename={0}'
            if show_content_in_browser:
                fileheader = 'inline; filename={0}'

            header_content = fileheader.format(filename)
            self['Content-Disposition'] = header_content
        else:
            del self['Content-Disposition']
```





## 其他人的下载方法

```python
#文件下载
def download(request):
    """                                                                          
    Send a file through Django without loading the whole file into               
    memory at once. The FileWrapper will turn the file object into an            
    iterator for chunks of 8KB.                                                  
    """ 
    
    #读取mongodb的文件到临时文件中
    fileid_=request.GET["fileid"]
    filepath_ = ('%s/%s'%(MEDIA_ROOT, fileid_)) #文件全路径
    file_=TFiles.objects.get(fileid=int(fileid_))
    filename_=file_.filename
    filetype_=file_.filetype
 
    if os.path.isfile(filepath_):
        pass
    else:
        mongoLoad(fileid_)
    
    #下载文件
    def readFile(fn, buf_size=262144):#大文件下载，设定缓存大小
        f = open(fn, "rb")
        while True:#循环读取
            c = f.read(buf_size)
            if c:
                yield c
            else:
                break
        f.close()
    response = HttpResponse(readFile(filepath_), content_type='APPLICATION/OCTET-STREAM') #设定文件头，这种设定可以让任意文件都能正确下载，而且已知文本文件不是本地打开
    response['Content-Disposition'] = 'attachment; filename='+filename_.encode('utf-8') + filetype_.encode('utf-8')#设定传输给客户端的文件名称
    response['Content-Length'] = os.path.getsize(filepath_)#传输给客户端的文件大小
    return response
```







## 参考

1. [django中文件下载（HttpResponse）](https://blog.csdn.net/w6299702/article/details/38777165)
2. [django-wkhtmltopdf](https://pypi.org/project/django-wkhtmltopdf/)

