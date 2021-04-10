---
title: 模型冲突问题解决
description: '-'
tags:
  - WEB开发
  - Web后端
  - Django
abbrlink: a832c4d
date: 2021-03-29 23:12:10
---



`python manage.py makemigrations` 会在`app/migrations`下生成迁移文件，`python manage.py  migrate` 会在数据库的`django_migrations`表添加上这条记录，如下图：

![1608519753135](http://blog.cdn.ionluo.cn/blog/1608519753135.png)



通常我们的`app/migrations`下的迁移文件为了统一会关联`git`，如此就保证了迁移文件的统一，如果执行`python manage.py  migrate`失败，可以看下数据库中`django_migrations`表是否和迁移文件匹配，把不匹配的删除即可。

