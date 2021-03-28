---
title: sessionStorage在firfox不能新开页面共享
description: 暂无描述！
tags:
  - WEB开发
  - Web前端
  - 兼容问题
abbrlink: 8338b07b
date: 2021-03-12 23:00:31
---



HTML5中的这个`sessionStorage`和传统后台的`session`并不完全是同一个东西，**主要是在多个标签页数据是否会共享的问题上的不同。**

**误区：**之前一直以为，只要会话还没有过期，不同标签页之间，相同域名下的sessionStorage是一样的。

**正确答案：**刷新当前页面，或者通过location.href、window.open、或者通过带target="_blank"的a标签打开新标签，之前的sessionStorage还在，但是如果你是主动打开一个新窗口或者新标签，新窗口（或标签）的`sessionStorage`的增删改和旧窗口已经没有关系了，如果只是在当前标签内跳转新页面（或者刷新），数据还会保留（前提当然是同域）。



> 摘自：https://mp.weixin.qq.com/s/wogEdUj9vPTLBzG_Xi1OKw



## 但是，

今天使用火狐浏览器，通过a标签新页面打开，发现新页面已经无法看到sessionStorage了。

![image-20210312153120056](http://blog.cdn.ionluo.cn/blog/image-20210312153120056.png)



我的FireFox版本：

![image-20210312153328735](http://blog.cdn.ionluo.cn/blog/image-20210312153328735.png)



**所以，以后使用sessionStorage时，只要打开新窗口就要特别注意了，新旧窗口数据不会互相同步。**建议使用localStorage。