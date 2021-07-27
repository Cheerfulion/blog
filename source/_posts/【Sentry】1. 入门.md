---
title: 2. 使用
description: '-'
tags:
  - WEB开发
  - Sentry
abbrlink: 8491dc1r
date: 2021-03-17 21:02:32
---

## 参考文档

https://docs.sentry.io/

http://sinhub.cn/2019/07/getting-started-guide-of-sentry/ (仅供参考，但是内容不完全对应我的情况，估计是版本问题)



sentry是基于Django开发的应用。



## 自托管sentry

**搭建方式：**

有下面两种方式，这里我选择了Docker安装：

- [通过 Docker 安装](https://docs.sentry.io/server/installation/docker/)
- [通过 Python 安装](https://docs.sentry.io/server/installation/python/)



**官方文档：**

https://develop.sentry.dev/self-hosted/



**我的环境：**

```bash
root@wen80:/home/cyagen# lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.04.2 LTS
Release:        18.04
Codename:       bionic
root@wen80:/home/cyagen# docker --version
Docker version 20.10.5, build 55c4c88
root@wen80:/home/cyagen# docker-compose -v
docker-compose version 1.28.5, build c4eb3a1f
```



**开始搭建：**

```bash
mkdir sentry && cd sentry

git clone https://github.com/getsentry/onpremise.git

# 过程中需要创建账号
cd onpremise/ && ./install.sh

docker-compose up -d

# 查看开启端口（默认是9000）
docker-compose ps -a
```

![image-20210311201511616](http://blog.cdn.ionluo.cn/blog/image-20210311201511616.png)

从上面可以看到，sentry是通过nginx容器代理访问的，对外端口是：`9000`。浏览器输入`服务器ip+端口号`即可看到如下页面：



![image-20210311202207822](http://blog.cdn.ionluo.cn/blog/image-20210311202207822.png)



若是首次登陆，之后会需要你配置具体的域名信息和邮箱信息，邮箱信息这一块下一节会具体详述，现在可以先用默认的即可。



配置完之后进可以进入Sentry了。

> 我这里首次登陆配置页面因为一些情况，没有看到是上面就回车了。所以我决定重新安装，步骤如下：
>
> > **2021-03-14更新**，下面不需要删除本地镜像，sentry的配置是在*onpremise*文件夹下的，需要重置sentry只需要删除所有容器，然后删除*onpremise*文件夹即可。
>
> ```bash
> # 停止容器
> docker-compose stop
> 
> # 删除容器(删除前需要确保容器没有在运行)
> docker-compose rm
> 
> # 删除所有本地镜像（注意：这里删除的是所有镜像，包括compose外面的，暂时不知道该如何只删除compose使用到的镜像）
> docker rmi `docker image -q` 
> 
> # 为了以防万一，我仓库也重新克隆
> cd ../ && rm -rf onpremise/
> 
> # 接上面安装的步骤（这里我不确定是镜像还是onpremise保存了配置，因为光删除容器是无法初始化sentry的）
> ```



**外部SMTP服务器邮件配置：**

自托管的Sentry附带有一个由[exim4](https://hub.docker.com/r/tianon/exim4)驱动的内置传出SMTP服务器。默认配置设置为使用此服务器。所有你需要做的是为设定一个有效的地址`mail.from`在`config.yml`。

如果要使用外部SMTP服务器，则可以`mail.*`在`config.yml`文件中设置相关设置，而忽略内置的SMTP服务器。

我这里使用内置的会报错，所以我使用外部SMTP服务器，步骤如下：

> 注意：下面用的25端口在网上购买的服务器上最好不要使用，具体请看`邮箱无法发送问题排查`一节

1. 修改`/sentry/config.yml`文件

   ```bash
   ###############
   # Mail Server #
   ###############
   
   mail.backend: 'smtp'  # Use dummy if you want to disable email entirely
   mail.host: 'smtp.mxhichina.com'
   mail.port: 25
   mail.username: 'ionluo@***.com'
   mail.password: '******'
   mail.use-tls: true
   # The email address to send on behalf of
   mail.from: 'ionluo@***.com'
   ```

2. 配置修改后 update 一下 Sentry 并重启

   ```bash
   docker-compose down
   docker-compose build
   docker-compose run --rm web upgrade
   docker-compose up -d
   ```

3. 测试是否成功

   重启后打开如下链接：http://127.0.0.1:9000/manage/status/mail/，即`Admin`下的Mail即可看到修改后的配置。点击“Test Settings”下的按钮即可测试，如果没有失败提示，那么之后你的邮箱就会收到测试邮件了。

   ![image-20210314164650901](http://blog.cdn.ionluo.cn/blog/image-20210314164650901.png)







## 页面无法打开问题排查

首先检查是否存在端口占用

然后正式服务器上需要服务商放行指定的端口（如默认的9000端口）



## 邮箱无法发送问题排查

首先确定Sentry版本，因为我看到网上博客配置都是五花八门的，估计是不同版本的原因。

1. 首先打开github查看tags，https://github.com/getsentry/onpremise/tags。

   我这里是`21.3.0`，所以版本号就是`21.3.0` （不绝对）

   如果需要完全确定，可以打开`http://[ip]:[端口]/manage/status/environment/`，里面就可以看到当前安装的版本了。

   ![image-20210316150157628](http://blog.cdn.ionluo.cn/blog/image-20210316150157628.png)

   

2. 版本号对上了，那么修改 `vim onpremise/sentry/config.yml`

   ```bash
   ###############
   # Mail Server #
   ###############
   mail.backend: 'smtp'  # Use dummy if you want to disable email entirely
   mail.host: 'smtp.mxhichina.com'
   mail.port: 25
   mail.username: 'ionluo@xxx.com'
   mail.password: 'xxxxx'
   mail.use-tls: true
   # The email address to send on behalf of
   mail.from: 'noreply@xxxx.com'
   ```

   修改好后需要重启，**不要忘记先关闭之前的容器**

   ```bash
   docker-compose down
   docker-compose build
   docker-compose run --rm web upgrade
   docker-compose up -d
   ```

   这里解决了我页面一直无法显示我的配置的问题，就是要先关闭容器。

   

3. [排查网络和端口的问题](http://lagel.me/server/sentry-mail.html)

   ```bash
   # 测试域名访问是否丢包
   ping smtp.mxhichina.com
   # 检查25端口是否可以连接上，正常的话会出现提示Escape character is '^]'.
   telnet smtp.mxhichina.com 25
   ```

   如果这两个都能够正常访问，那么跳过，这里是正常的。

   然而，我这里是能够ping通，但是 `telnet` 无法正常连上邮件服务器的`25`端口。但是阿里云的`465`端口是能够正常访问的。后来通过阿里云工单得知，2018年9月份之后购买的服务器的25端口是受限的。[详情查看](https://help.aliyun.com/knowledge_detail/40570.html?spm=5176.2020520165.120.d40570.2c837029DQW0LP#port25)

   所以，我们可以通过申请`25`端口解封来解决这个问题。或者配置sentry支持`ssl`。

   ```bash
   # 修改配置
   vim onpremise/sentry/config.yml
   ###############
   # Mail Server #
   ###############
   mail.backend: 'django_smtp_ssl.SSLEmailBackend'
   # mail.backend: 'smtp'  # Use dummy if you want to disable email entirely
   mail.host: 'smtp.mxhichina.com'
   mail.port: 465
   mail.username: 'ionluo@xxx.com'
   mail.password: 'xxxx'
   mail.use-tls: true
   # The email address to send on behalf of
   mail.from: 'noreply@xxxx.com'
   
   
   # sentry模块配置
   vim onpremise/sentry/requirements.txt
   # Add plugins here
   django-smtp-ssl==1.0
   
   
   # 修改socket超时时间
   vim onpremise/sentry/sentry.conf.py
   # 头部插入如下两行代码（默认事件是5s,容易导致timeout）
   import socket
   socket.setdefaulttimeout(20)
   
   cd onpremise
   # 接下来按照步骤2重启即可，docker启动会自动安装requirements.txt里面的包
   ```

4. 报错：`500， (440, b"mail from account doesn't conform with authentication (Auth Account:ionluo@xxx.com|Mail Account:noreply@xxxx.com)", 'noreply@xxxx.com')`

   原因是邮箱验证你指定的mail.from不属于你，防止其它账号打着自己的邮箱账号发送邮件？

   修改配置`onpremise/sentry/config.yml`里面的`mail.from`为上面的`mail.username`相同的账号即可。



## sentry设置https支持

通常正式站点都是需要支持https的，但是为了一个上报接口，专门创建一个域名和申购一个ssl证书比较麻烦。我这里使用一个有https证书的站点，使用nginx转发请求的方式绕过https限制。

2. nginx设置转发

   ```bash
   # nginx.conf
   # 这里是让所有以/sentry开头的请求转发到我们的http的站点。
   location ^~/sentry/ {
       rewrite ^/sentry/(.*)$ /$1 break;
       proxy_pass http://【ip】:9000;
   }
   ```

 3. 修改系统提供的dns链接, 规律如下：

    ```bash
    # 系统提供的
    http://xxxxxxxxxxxxxxxx@www.xxx.com:9000/2
    
    # 修改后的
    https://xxxxxxxxxxxxxxxx@www.xxx.com/sentry/2
    ```

> 不要直接修改根URL去适配页面，我们只需要修改上报接口去转发就好了，如果修改根URL(http://ip:9000/manage/settings/)，在用户确认邮件之后的页面就会出现页面无法加载的情况。
>
> 如下图：
>
> ![image-20210316170210124](http://blog.cdn.ionluo.cn/blog/image-20210316170210124.png)



## 相关文章推荐

1. https://blog.csdn.net/arnolan/article/details/105595994

2. http://lagel.me/server/sentry-mail.html

3. https://mp.weixin.qq.com/s?__biz=MzAwNTQ4NTcxMw==&mid=2247484953&idx=1&sn=0f6166f424a0e9231a261dfedd15bfa7&chksm=9b1aa3e7ac6d2af11071e5f05a4ad2b042c8143177d0cb35a742d15a1ea225d689262d538d9d&mpshare=1&scene=24&srcid=0903LVsbY7AZ18Y3zYZRIGxK&sharer_sharetime=1599094793832&sharer_shareid=8ec255e83b526f32a5c62680dcd9b96c&key=9255e861c291e6d52bdac7b8a1d4ea4f226d1bdbf0bc8d47ab7ea06c154275d50882cc0a077c35386fe94fead65df89a9b1a73e97ff592534c6cd50d8786518d5a89063a3aa89acf045123e8a2e368867e7b6dc8976532aff3f9ff6875ef676ad3f25957bc924b6c67aecb4e7ab1fd512d977f978be339320f16a07ba01c50d8&ascene=14&uin=MjUwMjUwMzQwOA%3D%3D&devicetype=Windows+10+x64&version=6300002f&lang=zh_CN&exportkey=A7wcWmk6sub4ye19fijDGd0%3D&pass_ticket=wA%2FnJqHkH9ybFue6Pz6YiaJHcWRGHW1ba7mcAdtrdWdObNq6AUIJSxF9ZEtuD0j3&wx_header=0

   > 最后一篇微信上的文章就是犯了使用25端口的错误，在通常的正式环境下，推荐还是使用ssl的465端口，因为一般服务运营商为了防止使用者搭建群发邮件的服务，都会禁掉25端口的。

4. [前端开发中的Error以及异常捕获](https://juejin.cn/post/6844903751271055374)