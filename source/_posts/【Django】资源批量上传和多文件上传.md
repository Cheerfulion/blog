---
title: 资源批量上传和多文件上传
description: '-'
tags:
  - 七牛云
abbrlink: 8ea810f0
date: 2021-03-12 23:00:31
---



## Python操作七牛云

我这里是django中的脚本：

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





**参考文章：**

https://developer.qiniu.com/kodo/kb/3741/how-to-bulk-delete-files-in-the-space

https://developer.qiniu.com/kodo/kb/1374/batch-upload-and-file-upload-more-resources