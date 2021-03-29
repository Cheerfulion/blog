---
title: 搭建记录
description: '-'
tags:
  - Rendertron
abbrlink: 70a15d1e
date: 2021-03-29 23:25:44
---



## 安装Chrome Headless

测试环境： Ubuntu 16.04

**安装**

如果是桌面版的ubuntu，直接到官网下载最新版chrome安装就好。对于服务器版的chrome，只能用命令行[安装服务器版本Chrome](https://askubuntu.com/questions/79280/how-to-install-chrome-browser-properly-via-command-line)

```bash
sudo apt-get install libxss1 libappindicator1 libindicator7
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome*.deb  # Might show "errors", fixed by next line
sudo apt-get install -f
```

> wget的下载地址因为是国外的，可能会无法连接，那么就用我下载好的deb文件：[google-chrome-stable_current_amd64.deb](http://blog.cdn.ionluo.cn/blog/google-chrome-stable_current_amd64.deb)
>
> 如果可以连接，但是因为网速慢经常断开重连，则给wget加上参数，不限制重连次数（默认20次）
>
> ```bash
> wget ––tries=0 https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
> ```
>
> 关于wget的给多用法，可以看：https://www.cnblogs.com/sx66/p/11887022.html

> `sudo dpkg -i google-chrome*.deb` 报错
>
> ![1607666886680](http://blog.cdn.ionluo.cn/blog/1607666886680.png)
>
> 解决方法：`sudo apt-get install -f`



**启动chrome**

```bash
 google-chrome --headless --remote-debugging-port=9222 https://www.baidu.com/ --disable-gpu --no-sandbox
```

> `--headless`： 使用headless模式，不开启的话使用MobaXterm远程连接会打开图形界面浏览器。
>
> `--disable-gpu`: ubuntu上大多没有gpu，所以--disable-gpu以免报错。
>
> `--no-sandbox`:  Running as root without --no-sandbox is not supported。报错信息是：`ERROR:zygote_host_impl_linux.cc(90)] Running as root without --no-sandbox is not supported. See https://crbug.com/638180.`应该是root用户才需要加的参数，使用沙盒模式运行。



**测试是否开启成功**

基本上看到下面就是开启成功了，如果不确定，新开一个终端，使用`curl http://localhost:9222`看是否能够拉下百度的html。

![1607667674864](http://blog.cdn.ionluo.cn/blog/1607667674864.png)



参考：https://www.jianshu.com/p/4ede64b7ccdb



## 安装python中间件版本的[rendertron](https://github.com/GoogleChrome/rendertron)

测试环境： Ubuntu 16.04, Django==1.8.4， python===2.7.12

**注意，使用python-rendertron需要python版本>3, 我这里条件不足，暂时放弃使用。(2020-12-11)**

由于公司的后台是用Django搭建的，因此使用django的中间件。

**安装**

```bash
pip install rendertron
```

> 我这里遇到了如下报错：
>
> `ERROR: Could not find a version that satisfies the requirement rendertron (from versions: none)
> ERROR: No matching distribution found for rendertron`
>
> ![1607668615695](http://blog.cdn.ionluo.cn/blog/1607668615695.png)
>
> 该问题没有找到解决方案，网上说的使用代理源，升级pip版本，重试多几次都无效，我猜测是python版本太低的原因（python2.7.12）,先继续看看行不行。

使用下面命令安装最新的开发版本

```bash
pip install -e git://git@github.com:frontendr/python-rendertron.git@develop#egg=rendertron
```

> 发现也安装不了。。。
>
> 因此先下载该git仓库再安装
>
> ```bash
> git clone https://github.com/frontendr/python-rendertron.git
> pip install -e python-rendertron/
> ```
>
> 报错：`ERROR: Package 'rendertron' requires a different Python: 2.7.12 not in '>=3'`
>
> 如此，在公司旧后台使用python2.7的项目就**无法使用**rendertron了。
>
> **扩展**：pip install -e 用于安装本地项目，会自动将包复制到site-packages
>
> > Using `pip install -e .` can actually be useful if you want to run your packages with `python package.py` and you import other modules of your project from that file. The command makes them findable!
> >
> > What it does is:
> >
> > - installs `site-packages/PackageName.egg-link` file
> > - adds path to `site-packages/easy-install.pth`
> > - optionally installs CLI targets in `<venv>/bin`
> >
> > It seems either of the earlier two is sufficient, and the latter is handy when developing command-line utilities.
> >
> > --- 摘自：https://stackoverflow.com/questions/42609943/what-is-the-use-case-for-pip-install-e/59667164#59667164?newreg=9c456c4fac1e46049b0174b263f67d0b  [Dima Tisnek]



## 安装Express.js中间件版本的[rendertron](https://github.com/GoogleChrome/rendertron)

测试环境： Ubuntu 16.04, Django==1.8.4， python===2.7.12





## 参考

https://juejin.im/post/6844904024374771720

https://github.com/GoogleChrome/rendertron#installing--deploying

https://github.com/frontendr/python-rendertron

https://github.com/GoogleChrome/rendertron