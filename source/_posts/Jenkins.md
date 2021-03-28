---
title: Jenkins
description: 暂无描述！
tags:
  - Jenkins
abbrlink: 9a7f448e
date: 2021-03-28 18:18:57
---



## 查看系统是否安装java

```shell
java -version
```

![1603967039322](http://blog.cdn.ionluo.cn/blog/1603967039322.png)

如果是上面的情况，就需要安装java了

如果不是，而是下面一样显示了java的版本，那么就可以跳过安装java的步骤了。

![1603967111643](http://blog.cdn.ionluo.cn/blog/1603967111643.png)

> 扩展：`which java`从环境变量里面找java
>
> `whereis java`从系统文件索引里面找java
>
> `find / -name java`文件搜索查找java



## 安装java

> java的安装与介绍：https://www.osetc.com/archives/20430.html#linux-jdk-openjdk-difference

其他方法可以参考：https://blog.csdn.net/zbj18314469395/article/details/86064849

1、更新软件包列表：

```bash
sudo apt-get update
```

2、安装openjdk-8-jdk：

```bash
sudo apt-get install default-jre
```

3、查看java版本，看看是否安装成功：

```bash
java -version
```

![1603967111643](http://blog.cdn.ionluo.cn/blog/1603967111643.png)

> 删除软件：`sudo apt-get remove 软件名`
>
> 如果不清楚软件名用：`dpkg --get-selections | grep xxxx` 来查找软件



### 下载并运行 Jenkins

1. 打开终端进入到下载目录， 运行命令下载 Jenkins文件

   方法1：**curl**（会报错，具体原因不详）https://www.linuxprobe.com/chapter-04.html

   ```shell
   curl -# -O http://mirrors.jenkins.io/war-stable/latest/jenkins.war
   ```

   ![1604019904605](http://blog.cdn.ionluo.cn/blog/1604019904605.png)

   

   方法2：**wget**  https://www.cnblogs.com/pretty-ru/p/10936023.html

   ```shell
   wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
   ```

   

   方法3：**ftp**、**scp**  https://www.jb51.net/article/82609.htm

   ```shell
   # 本地发远程
   scp /home/ionluo/jenkins.war root@172.19.2.75:/home/ionluo/soft-package
   # 远程发本地
   scp root@172.19.2.75:/home/ionluo/soft-package /home/ionluo/jenkins.war
   ```

2. 运行命令 `java -jar jenkins.war --httpPort=8080`.

   ![1604021001753](http://blog.cdn.ionluo.cn/blog/1604021001753.png)

   我这里提示端口被占用，可以通过下面方式查找被谁占用

   ```shell
   lsof -i:8080
   ```

   ![1604021080214](http://blog.cdn.ionluo.cn/blog/1604021080214.png)

   可以看到是nginx占用了，因此不能杀掉nginx的进程，只能用新端口打开了`java -jar jenkins.war --httpPort=8888`。

   如果需要杀掉占用端口的进程，可以用`kill -9 [PID]`的方式强制杀死该进程。如果不加`-9`表示发送信号给进程，请进程自行停止运行并退出。

3. 打开浏览器进入链接 `http://localhost:8080`.

4. 按照说明完成安装.

   这里作为一个新手，使用“安装推荐的插件”，这时候会自动安装jenkins社区推荐的一系列插件。在我的阿里云服务器上有下载慢的情况，可以尝试[jenkins按照插件很慢，提速的解决方法，亲测有效](https://blog.csdn.net/u013788943/article/details/103822785)下这个博客的方法（未测试）

   ![1604021784147](http://blog.cdn.ionluo.cn/blog/1604021784147.png)

   ![1604022290241](http://blog.cdn.ionluo.cn/blog/1604022290241.png)

   > 2020-12-28  账密修改为 【ion 】【ionluo】
   >
   > 2020-12-29  账密修改为 【ion】【cyagenB15】
   
   ![1604022339514](http://blog.cdn.ionluo.cn/blog/1604022339514.png)



## Jenkins使用

1. 新建任务 》 构建一个自由风格的软件项目

   ![1609152606650](http://blog.cdn.ionluo.cn/blog/1609152606650.png)

2. 配置

   ![1609152722921](http://blog.cdn.ionluo.cn/blog/1609152722921.png)

   ![1609152749233](http://blog.cdn.ionluo.cn/blog/1609152749233.png)

   ![1609152763571](http://blog.cdn.ionluo.cn/blog/1609152763571.png)

   构建脚本如下：

   ```bash
   # 判断Git是否有新的提交(Jenkins可用环境变量法，截图位置可以看到这个，点开“查看可用环境变量列表”)
   if [ ! $GIT_PREVIOUS_SUCCESSFUL_COMMIT ];then
       echo "GIT_PREVIOUS_SUCCESSFUL_COMMIT is not exists."
       exit 0
   else
       echo "GIT_COMMIT=[$GIT_COMMIT],GIT_PREVIOUS_SUCCESSFUL_COMMIT=[$GIT_PREVIOUS_SUCCESSFUL_COMMIT]"
       if [ $GIT_PREVIOUS_SUCCESSFUL_COMMIT == $GIT_COMMIT ];then
           echo "GIT_COMMIT is equals to GIT_PREVIOUS_SUCCESSFUL_COMMIT,skip build."
           exit -1
       else
           echo "GIT_COMMIT is not equals to GIT_PREVIOUS_SUCCESSFUL_COMMIT"
           # 防止Ubuntu高版本默认使用dash执行脚本
           /bin/bash /root/ionluo/scripts/web_cyagen_net.sh
           exit 0
       fi
   fi
   ```
   
   服务器上的脚本如下：
   
   ```bash
   #!/bin/sh
   
   # 出现一个异常语句会中断后续操作（缺点是无法定制错误信息，正式环境下开启这个会好点，这样子就不需要设置下面的函数error_exit处理错误了）
   set -e
   
   #执行出错的处理函数
   #function error_exit {
   #  echo "$1" 1>&2
   #  exit 1
   #}
   # cd /var/www/web_cyagen_net/ || error_exit "$LINENO: cd failed"
   
   # 项目位置
   cd /var/www/web_cyagen_net/
   
   # 判断Git是否有新的提交(git原始命令判断法)
   # 关于Git的refs可以看这篇文章 https://segmentfault.com/a/1190000007996197
   # echo "Remote version: $(git ls-remote origin refs/heads/master)"
   # echo "Local version: $(git rev-parse FETCH_HEAD)"
   # echo "Remote origin url: $(git ls-remote --get-url)"
   
   git pull
   git checkout master
   # git reset --hard origin/master  # 强制保持和远程master分支代码相同
   
   source /var/www/env/web/bin/activate
   
   # 费时操作根据判断是否执行
   if [ $(git diff HEAD HEAD~1 --stat | grep -c "requirements.txt") -ge 1 ]; then
   	pip install -r ./requirements.txt
   fi
   
   if [ $(git diff HEAD HEAD~1 --stat | grep -c "app/migrations") -ge 1 ]; then
   	python manage.py migrate --database=default --noinput
   fi
   
   if [ $(git diff HEAD HEAD~1 --stat | grep -c "locale/") -ge 1 ]; then
   	python manage.py compilemessages
   fi
   
   # 这个前端代码打包操作不适合放到服务器进行
   if [ $(git diff HEAD HEAD~1 --stat | grep -c -E "\.js|\.css|\.less") -ge 1 ]; then
       cd app/static/
       # 显示当前文件夹下的文件(-d), 并按时间排序(-t)
       # 从第5个位置开始输出
       # 删除文件夹
       ls -dt ./build/*/ | tail -n +5 | xargs rm -rf
       grunt build
       git rev-parse --short FETCH_HEAD > ../../VER
       cd ../../
   fi
   
   supervisorctl restart web
   python manage.py clear_cache
   
   deactivate  
   ```
   
3. 保存后，gitlab中设置回调钩子

   > 下面截图有个地方需要补充下，**Push events**下的框输入触发的分支，不填是默认所有分支的push都会触发回调

   ![1609153250014](http://blog.cdn.ionluo.cn/blog/1609153250014.png)

4. 修改代码，push到master分支后，就会看到下面的trigger是否成功了。这里花了我最多的时间才解决。具体可见问题记录中的  **webhook回调报错**

   ![1609153291979](http://blog.cdn.ionluo.cn/blog/1609153291979.png)

## Jenkins保持一直运行

1. **nohup** （简单，相当于后台运行，容易忘记不使用的时候关闭）

   ```bash
   BUILD_ID=dontKillMe nohup java -jar jenkins.war --httpPort=8010
   
   # 关闭需要执行以下命令
   # lsof -i :8010
   # kill -9 [pid]
   ```

2. **supervisor** （推荐，该方式更好管理进程）

   supervisor的安装和使用这里不过多介绍了，上配置文件：

   ```bash
   [program:jenkins]
   directory= /root/ionluo/soft-package/
   command = /usr/bin/java -jar /root/ionluo/soft-package/jenkins.war --httpPort=8888
   user = root
   autostart = true
   autorestart = true
   stopsignal = QUIT
   #redirect_stderr = true
   #stdout_logfile = /var/log/supervisor/jenkins.log
   #stderr_logfile = /var/log/supervisor/jenkins_err.log
   #logfile_maxbytes = 1M
   ```

   > 值得一提的是错误：supervisor ERROR (spawn error)
   >
   > 排错使用命令：supervisorctl tail [programname] stdout
   >
   > 详见：https://blog.csdn.net/sinat_21302587/article/details/76836283

3. **init.d、systemd设置开机启动**

   【略】

   

## 问题记录

### 忘记管理员密码怎么办

https://jingyan.baidu.com/article/86f4a73ed42dbd37d65269a4.html (未验证)

>  如果是root用户，Jenkins目录位置在：/root/.jenkins



### 脚本执行报错`source: not found`

参考：https://www.cnblogs.com/PhoenixMY/p/5537928.html

```
现象： shell脚本中source aaa.sh时提示 source: not found

原因： ls -l `which sh` 提示/bin/sh -> dash

这说明是用dash来进行解析的。

改回方法： 

命令行执行：sudo dpkg-reconfigure dash

在界面中选择no

再ls -l `which sh` 提示/bin/sh -> bash
```

需要注意的是，如果对修改后的后果不太了解可以参考：https://blog.csdn.net/weixin_39212776/article/details/81097990

如果不是你一个人管理的服务器，建议还是不要随便改，强制使用bash去解析脚本即可，执行`/bin/bash aaa.sh`

### webhook回调报错

1. `Error 403 No valid crumb was included in the request`

   ```
   前言
   
   根据官网描述，Jenkins版本自2.204.6以来的重大变更有：删除禁用 CSRF 保护的功能。 从较旧版本的 Jenkins 升级的实例将启用 CSRF 保护和设置默认的发行者，如果之前被禁用。
   
   虽然删除了禁用csrf保护功能，增加了安全性，但是在一些结合Gitlab、Spinnaker等等工具进行持续集成过程中就增加了一些认证环节，若没有进行相关配置，得到的一定是403的报错。因为集成服务都是在内网操作，为删繁就简，笔者便考虑关闭 CSRF 保护功能，于是乎，对此展开了摸索……
   
   方案
   
   老版本Jenkins的CSRF保护功能只需要在 系统管理 > 全局安全配置 中便可进行打开或者关闭。让人头疼的是较高版本的Jenkins竟然在管理页面关闭不了CSRF，网上搜索到的资料有写通过 groovy代码 实现取消保护，但是笔者操作未成功，最后，Get到了一种成功的解决姿势。
   
   在Jenkins启动前加入相关取消保护的参数配置后启动Jenkins，即可关闭CSRF，配置内容如下：
   
   -Dhudson.security.csrf.GlobalCrumbIssuerConfiguration.DISABLE_CSRF_PROTECTION=true
   Jenkins若是跑在Tomcat下，只需在tomcat启动脚本中加入配置即可；若是以jar包形式部署的，只需在启动时加上配置参数即可。
   
   上面摘自： https://issues.jenkins-ci.org/browse/JENKINS-61375
   
   因此，修改后的supervisor启动命令修改为：
   command = /usr/bin/java -jar -Dhudson.security.csrf.GlobalCrumbIssuerConfiguration.DISABLE_CSRF_PROTECTION=true /root/ionluo/soft-package/jenkins.war --httpPort=8888
   ```

   修改后登陆，“系统管理”下的“全局安全配置”的“跨站请求伪造保护”就不再显示了。![1609149022341](http://blog.cdn.ionluo.cn/blog/1609149022341.png)

   > 另外看到有这个方式，但是没有找到jenkins控制台
   >
   > https://www.cnblogs.com/handsomejunhong/p/13590566.html

2. `You are authenticated as: anonymous`

   Jenkins 系统设置中设置未授权用户可以读访问。

   ![1609151745481](http://blog.cdn.ionluo.cn/blog/1609151745481.png)

   ![1609151758689](http://blog.cdn.ionluo.cn/blog/1609151758689.png)

   > 这里着实让人疑惑，那我在构建触发器的时候的身份验证令牌是第二重验证吗？需要登录验证再加触发器验证？
   >
   > ![1609151896360](http://blog.cdn.ionluo.cn/blog/1609151896360.png)



 ### 安装插件报错`java.net.SocketTimeoutException: Read timed out`

![1610350670717](http://blog.cdn.ionluo.cn/blog/1610350670717.png)



解决方法：1. 替换源

```
# 参考： https://www.cnblogs.com/testway/p/6387307.html

1. jenkins->系统管理->管理插件->高级
2. 选择“升级站点”

# 镜像地址查询：
# http://mirrors.jenkins-ci.org/status.html

3. 把https://updates.jenkins.io/update-center.json换成：https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

2. 上面提到的未尝试的方法

3. 手动下载插件上传

   jenkins->系统管理->管理插件->高级->上传插件





## 脚本学习记录

https://www.jb51.net/article/100490.htm

https://www.cnblogs.com/clsn/p/8028337.html#auto-id-41

## Jenkins学习记录

https://www.cnblogs.com/wfd360/p/11314697.html