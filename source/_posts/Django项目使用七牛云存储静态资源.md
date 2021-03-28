---
title: Django项目使用七牛云存储静态资源
description: 暂无描述！
tags:
  - WEB开发
  - Web后端
  - Django
abbrlink: 5e2a48c7
date: 2021-03-12 23:00:31
---



## 前言

环境：`Django==1.8.4` `django-qiniu-storage==2.1.1` 

文档：[Django Qiniu Storage](https://django-qiniu-storage.readthedocs.io/zh_CN/latest/)



## 注册七牛云账号

注册好账号后，1、 在`个人中心 > 密钥管理` 里面找到自己的`AccessKey/SecretKey`。

2、在`对象存储 > 空间管理` 里面`新建空间`， 得到`空间名、空间域名`。这样子就拿到需要的四个配置的参数。

```shell
# AccessKey
QINIU_ACCESS_KEY
# SecretKey
QINIU_SECRET_KEY
# 空间名
QINIU_BUCKET_NAME
# 空间域名(新建空间会自动分配一个30天有效期的域名，之后会回收，也可以绑定自己的域名)
QINIU_BUCKET_DOMAIN
```



## 配置Django项目

1. **安装七牛云的包**

   ```bash
   pip install django-qiniu-storage
   ```

2. **配置`setting.py`**

   ```python
   QINIU_ACCESS_KEY = 'Your AccessKey'
   QINIU_SECRET_KEY = 'Your SecretKey'
   QINIU_BUCKET_NAME = 'Your Bucket Name'
   QINIU_BUCKET_DOMAIN = 'Your Bucket Domain'
    
   MEDIA_URL = QINIU_BUCKET_DOMAIN + '/media/'
   STATIC_URL = QINIU_BUCKET_DOMAIN + '/static/'
   
   DEFAULT_FILE_STORAGE = 'qiniustorage.backends.QiniuMediaStorage' 
   STATICFILES_STORAGE = 'qiniustorage.backends.QiniuStaticStorage'
   ```

3. **运行命令上传文件到七牛云**

   ```shell
   python manage.py collectstatic
   ```

   如此，STATIC和MEDIA文件都会上传到七牛云上。

   > 上面设置需要修改MEDIA_URL和STATIC_URL来使得页面上的文件URL是七牛云的域名，实际上也可以自定义STORAGE类来自定义URL，如此可以通过一个变量USE_CDN来控制是否需要上传静态文件。
   >
   >  ```python
   >    # settings.py
   >    QINIU_ACCESS_KEY = 'Your AccessKey'
   >    QINIU_SECRET_KEY = 'Your SecretKey'
   >    QINIU_BUCKET_NAME = 'Your Bucket Name'
   >    QINIU_BUCKET_DOMAIN = 'Your Bucket Domain'
   >     
   >    MEDIA_URL = '/media/'
   >    STATIC_URL = '/static/'
   >    
   >    DEFAULT_FILE_STORAGE = 'app.helper.my_qiniu.MyQiniuMediaStorage'
   >    STATICFILES_STORAGE = 'app.helper.my_qiniu.MyQiniuStaticStorage'
   >    
   >    USE_CDN = True
   >  ```
   >
   >  
   >
   >    ```python
   >    # app/helper/my_qiniu.py
   >    
   >    import re
   >    from django.conf import settings
   >    
   >    class MyQiniuMediaStorage(QiniuMediaStorage):
   >        def url(self, name):
   >            if not re.match('media/(.*)', name):
   >                name = self._normalize_name(self._clean_name(name))
   >            else:
   >                name = self._clean_name(name)
   >            return  urljoin(settings.PROTOCOL + self.bucket_domain, name) if settings.USE_CDN else name
   >    
   >    
   >    class MyQiniuStaticStorage(QiniuStaticStorage):
   >        def url(self, name):
   >            name = self._normalize_name(self._clean_name(name))
   >            return  urljoin(settings.PROTOCOL + self.bucket_domain, name) if settings.USE_CDN else name
   >    ```

  

## 问题处理

1.  **使用python mangae.py collectstatic 400报错，报错信息如下：**

   ```bash
   qiniustorage.utils.QiniuError: exception:None, status_code:400, _ResponseInfo__response:<Response [400]>, text_body:{"error":"incorrect region, please use up-na0.qiniup.com"}, req_id:vusAAACUXt1b8mUW, error:incorrect region, please use up-na0.qiniup.com, x_log:X-Log
   ```

   **文档：**https://developer.qiniu.com/kodo/kb/3742/upload-to-400:-incorrect-region-both-please-use-xxxx-qiniu-com

   **解决：**`django-qiniu-storage`插件默认的区域是`华东区域`的接口，在`settings.py`里面配置成存储空间所在区域的接口即可，下面以`北美地区`为例。

   ```python
   from qiniu.config import set_default
   set_default(default_up_host='up-na0.qiniup.com')
   ```

2. 设置默认存储是七牛云后，会导致查询文件速度慢

   ```python
   content_type = ContentType.objects.get_for_model(bundle.obj) 
   
   for f in MyUploads.objects.filter(object_id=bundle.obj.pk, content_type=content_type):
       b = UploadsResource().build_bundle(obj=f.upload, request=bundle.request)
       upload_bundle = UploadsResource().full_dehydrate(b, for_list=True)
       upload_bundle.data['category'] = f.category
       upload_bundle.data['name'] = f.name or f.upload.file_name
       upload_bundle.data['keywords'] = f.keywords
       bundle.data['files'].append(upload_bundle)
   ```
   
   **解决方法：**
   
   一开始以为是ContentType关联速度慢导致，但是后来发现，taspie执行full_dehydrate时，读取文件的大小和宽高，而数据库中，Upload的file其实只是存储了文件的位置，执行这些操作是需要先从七牛云下载文件的，这也就是造成速度慢的原因，也就是说，实际上只有self.file.url是从数据库读取的。因此不建议查询的时候去获取文件的其他信息。或者建立一些其他字段，在上传的时候就保存好这些信息。

## 扩展

django中脚本：

依赖：`qiniu`和`qiniustorage`

更多qiniu的脚本用法见：https://developer.qiniu.com/insight/4662/sdk

```python
# -*- coding: utf-8 -*-
import logging
import re
from urlparse import urljoin
import os
import datetime

from qiniustorage.backends import QiniuMediaStorage, QiniuStaticStorage
from qiniu import Auth, BucketManager, build_batch_stat, put_file, build_batch_delete
from qiniu.utils import etag
from django.conf import settings

logger = logging.getLogger('qiniu')

class MyQiniu(object):
    def __init__(self, access_key=None, secret_key=None, bucket_name=None):
        """"""
        self.access_key = access_key if access_key else settings.QINIU_ACCESS_KEY
        self.secret_key = secret_key if secret_key else settings.QINIU_SECRET_KEY
        self.bucket_name = bucket_name if bucket_name else settings.QINIU_BUCKET_NAME

        self.q = Auth(self.access_key, self.secret_key)
        self.bucket = BucketManager(self.q)

        self.remote_file_list = []
        self.local_file_list = []

    def init_list(self):
        """ 初始化本地文件列表和远程文件列表 """
        begin_tm = datetime.datetime.now()
        self.remote_file_list = self.get_remote_file_list()
        self.local_file_list = self.get_local_file_list()
        init_tm = datetime.datetime.now()
        logger.info('Init list: {} seconds, remote files: {}, local files: {}'.format(
            (init_tm - begin_tm).seconds, len(self.remote_file_list), len(self.local_file_list)
        ))

    def is_need_upload(self, _key, _hash):
        result = True
        for f in self.remote_file_list:
            if f['key'] == _key and f['hash'] == _hash:
                result = False
                break

        return result

    def is_need_delete(self, _key):
        result = True
        if re.search('media/', _key, re.I):
            result = False
        else:
            for f in self.local_file_list:
                if f['key'] == _key:
                    result = False
                    break
        return result

    def build_batch_stat(self, file_list):
        """ 批量获取文件信息
        :rtype : ret, info
        """
        ops = build_batch_stat(self.bucket_name, file_list)
        return self.bucket.batch(ops)

    def get_remote_file_list(self, prefix=None, limit=None):
        result = []
        marker = None
        eof = False
        while eof is False:
            ret, eof, info = self.bucket.list(self.bucket_name, prefix=prefix, marker=marker, limit=limit)
            marker = ret.get('marker', None)
            for item in ret['items']:
                if item['key']:
                    result.append(item)
        if eof is not True:
            raise Exception('Get remote files error!')
        return result

    def get_local_file_list(self, static_path=None, exclude=None):
        if not static_path:
            static_path = os.path.join(settings.PROJECT_PATH, 'app', 'static')
        if not exclude:
            exclude = ['node_modules', '.grunt', 'panorama']
            # http://stackoverflow.com/questions/19859840/excluding-directories-in-os-walk
            # http://stackoverflow.com/questions/5141437/filtering-os-walk-dirs-and-files

        result = []
        for root, dirs, files in os.walk(static_path, topdown=True):
            dirs[:] = [d for d in dirs if d not in exclude]

            for f in files:
                if not re.search('^\.', f):
                    _path = os.path.join(root, f)
                    result.append({
                        'key': re.sub('^' + static_path, 'static', _path),
                        'hash': etag(_path)
                    })

        if len(result) < 100:
            raise Exception('number of files < 100')
        return result

    def get_upload_list(self):
        result = []
        for f in self.local_file_list:
            if self.is_need_upload(f['key'], f['hash']):
                result.append(f['key'])
        return result

    def get_delete_list(self):
        result = []
        for f in self.remote_file_list:
            if f['key'] and self.is_need_delete(f['key']):
                result.append(f['key'])
        return result

    def upload_file(self, key):
        """

        :type key: 文件相对路径, 比如 /var/www/web/app/static/bower.json 要写成 static/bower.json
        :rtype : ret, info
        """
        local_file = os.path.join(settings.PROJECT_PATH, 'app', key)
        token = self.q.upload_token(self.bucket_name, key)
        return put_file(token, key, local_file, check_crc=True)

    def batch_delete(self, file_list):
        """  

        :rtype : ret, info
        """
        # 每次删除 1000
        for i in range(0, len(file_list), 1000):
            ops = build_batch_delete(self.bucket_name, file_list[i:i + 1000])
            ret, info = self.bucket.batch(ops)
            logger.info(ret)

        return
            # return self.bucket.batch(ops)

    def sync(self):
        begin_tm = datetime.datetime.now()
        delete_list = self.get_delete_list()
        delete_list_tm = datetime.datetime.now()
        logger.info(
            'Get delete list: {} seconds, number of files: {}'.format(
                (delete_list_tm - begin_tm).seconds, len(delete_list)
            )
        )

        upload_list = self.get_upload_list()
        upload_list_tm = datetime.datetime.now()
        logger.info(
            'Get upload list: {} seconds, number of files: {}'.format(
                (upload_list_tm - delete_list_tm).seconds, len(upload_list)
            )
        )

        for key in upload_list:
            logger.info('Uploading {}'.format(key))
            self.upload_file(key)

        logger.info('Delete files...')
        self.batch_delete(delete_list)
        return

    def upload_big_file(self):
        local_file = ''
        key = ''
        print key
        token = self.q.upload_token(self.bucket_name, key)
        ret, info = put_file(token, key, local_file)
        
        
def run():
    qiniu = MyQiniu()
    
    # 同步本地和存储空间里面的文件
    qiniu.init_list()
    qiniu.sync()
    
    # 删除存储空间里面的所有文件
    remote_list = qiniu.get_remote_file_list()
    remote_list = [file['key'] for file in remote_list]
    qiniu.batch_delete(remote_list)
    
    print 'Done.'
    
if __name__ == "__main__":
    run()
```

