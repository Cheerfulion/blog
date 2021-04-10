---
title: django随笔记录
description: '-'
tags:
  - WEB开发
  - Web后端
  - Django
abbrlink: 25abee52
date: 2021-03-29 23:10:43
---



## 部署（nginx + uwsgi socket ）

### 检查开发服务器下是否正常能够运行网站

### 问题记录

1. `supervisord -c /etc/supervisor/supervisord.conf`报错

   ![1604977533436](http://blog.cdn.ionluo.cn/blog/1604977533436.png)

   **解决：**

   优先参考这篇文章：http://siyouku.cn/article/6895.html

   `supervisorctl reread`   重新读取配置

   `supervisorctl update`   更新配置

   `supervisorctl start all`   开始所有配置

   >  我这里用了csdn找的 `unlink /var/run/supervisor.sock`导致了下面的报错。

2. 启动supervisor报错，`unix:///var/run/supervisor.sock no such file`

   ![1604977607347](http://blog.cdn.ionluo.cn/blog/1604977607347.png)

   github上说到可以用这个方式（未验证）：

   ```shell
   sudo touch /var/run/supervisor.sock
   sudo chmod 777 /var/run/supervisor.sock
   sudo service supervisor restart
   ```

   > 实际上我只用了 `sudo service supervisor restart`就生效了

3. 启动后报错`ERROR: Process web_old_rq: spawn error`

   **解决：**可以用 `supervisorctl tail program_name stderr` 或者 `supervisorctl tail program_name stdout`查看日志，就可以看到配置中对应的日志错误文件

   ![1604978655792](http://blog.cdn.ionluo.cn/blog/1604978655792.png)

   > 这里提示是不存在日志文件，但是创建了还是报错，所以问题未解决





## 常用插件

### django-debug-toolbar

#### 问题记录

**1. 访问速度慢**

启用debug_toolbar后网页加载变得很慢，这是因为debug_toolbar需要jquery文件，默认情况下debug_toolbar会从google引用jquery，这就很尴尬了，解决方法如下。

下载`jquery-3.3.1.min.js`，然后放到项目的static静态资源文件夹下面，然后在setttings.py中：

```python
# JQUERY_URL对应的地址映射到项目中jquery文件的位置
DEBUG_TOOLBAR_CONFIG = {'JQUERY_URL': '/static/js/jquery-3.3.1.min.js'}
```

**2. `nginx`转发请求后`debug_toolbar`不显示**

使用`nginx`转发请求后，`debug_toolbar`加载不出来，这是因为`nginx`会从项目的静态资源文件中引用`debug_toolbar`需要的静态资源，而这些资源却是放在`debug_toolbar`的安装目录下面的，这时我们需要把静态资源添加到项目的静态资源中。

`debug_toolbar`的静态资源文件在虚拟环境的`/usr/local/lib/[python版本]/dist-packages/debug_toolbar/static`或者`[虚拟环境名]/lib/[python版本]/site-packages/debug_toolbar`文件夹下，把这个文件夹下的`debug_toolbar`文件夹复制到项目的static静态资源文件夹下，问题就解决了。



### django-redis

https://www.jb51.net/article/141584.htm

https://blog.csdn.net/jack_970124/article/details/103142637

