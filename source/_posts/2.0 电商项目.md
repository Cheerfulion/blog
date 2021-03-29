---
title: 2.0 电商项目
description: '-'
tags:
  - WEB开发
  - Web前端
  - vue
abbrlink: 35066d2d
date: 2021-03-29 22:17:33
---







## 目录

![1597673839724](http://blog.cdn.ionluo.cn/blog/1597673839724.png)

## 项目概述

![1597673608523](http://blog.cdn.ionluo.cn/blog/1597673608523.png)

![1597673656264](http://blog.cdn.ionluo.cn/blog/1597673656264.png)

![1597673543115](http://blog.cdn.ionluo.cn/blog/1597673543115.png)

![1597673774354](http://blog.cdn.ionluo.cn/blog/1597673774354.png)

## 项目初始化

  ![1597761159662](http://blog.cdn.ionluo.cn/blog/1597761159662.png)

这里暂不打算使用后台服务，因为视频课程并没有后台源码，这里用模拟数据代替(mock.js)。

![1597762573944](http://blog.cdn.ionluo.cn/blog/1597762573944.png)

![1597762944507](http://blog.cdn.ionluo.cn/blog/1597762944507.png)

## 登录/退出功能

### 登录业务流程

**如果客户端与服务器端不存在跨域，可以使用cookie+session的登录方案**

**如果客户端与服务器端存在跨域，则要使用token的方式维持登录状态。**

![1597763382290](http://blog.cdn.ionluo.cn/blog/1597763382290.png)



![1597763553038](http://blog.cdn.ionluo.cn/blog/1597763553038.png)

![1597764557923](http://blog.cdn.ionluo.cn/blog/1597764557923.png)



字体图标使用阿里云的`IconFont`， 选好图标后点击下载代码，根据下载的文件操作引入项目中就好了。

> 需要注意的是安装新的依赖后要重启app才可以访问

登录这里通过token保存sessionStorage的方式维持登录状态，该方式的缺陷是无法设置过期时间，浏览器关闭后就会清空了。这里推荐还是保存cookie中会好些，当然也有安全问题，需要配合服务器端处理。如果需要根据权限动态生成路由，还需要保存动态生成的路由到sessionStorage中，并通过addRouter方法动态添加路由。





### 路由导航守卫控制访问权限

![1597939837054](http://blog.cdn.ionluo.cn/blog/1597939837054.png)

### 退出功能实现原理

![1597940604461](http://blog.cdn.ionluo.cn/blog/1597940604461.png)

## 主页布局

![1598017680914](http://blog.cdn.ionluo.cn/blog/1598017680914.png)

## 左侧菜单布局

![1598069002161](http://blog.cdn.ionluo.cn/blog/1598069002161.png)

### 接口获取菜单项

![1598070467507](http://blog.cdn.ionluo.cn/blog/1598070467507.png)





除了登录接口，其他接口都需要进行授权才可以调用

![1598070592911](http://blog.cdn.ionluo.cn/blog/1598070592911.png)





视频里面获取当前的路由地址来显示激活的菜单项高亮，用了sessionStorage存储的方式，但是这里直接绑定成`:default-active='$router.path'`更直接简洁





## 权限管理

![1598542524134](http://blog.cdn.ionluo.cn/blog/1598542524134.png)

接口：获取角色列表

![1598542574742](http://blog.cdn.ionluo.cn/blog/1598542574742.png)接口： 角色授权

![1598688880121](http://blog.cdn.ionluo.cn/blog/1598688880121.png)