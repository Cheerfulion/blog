---
title: 【web安全】Web前端黑客技术揭秘（学习笔记）
description: '-'
tags:
  - WEB开发
  - Web安全
abbrlink: 316b2285
date: 2021-01-07 21:05:03
---





## 前言

这是我的《Web前端黑客技术揭秘》的学习记录。书籍电子版





## 摘要

列举书中学习到的一些知识点，汇总在这里方便直接查看



**点击劫持（clickjacking）**

点击劫持（clickjacking）是一种视觉上的欺骗，攻击者把一个透明的 iframe 覆盖在目标网页的某个位置，这个位置可以是一个按钮，一段文字或一张图片等，诱使用户点击。

防范方法：限制网站的 iframe 来源。通过在 HTTP 响应报文中添加 X-Frame-Option 首部， 就能让浏览器按照要求加载 iframe 的页面。

```bash
# nginx(可以置于http server header中)

# 正常情况下都是使用SAMEORIGIN参数，允许同域嵌套
add_header X-Frame-Options SAMEORIGIN;
# 禁止嵌套
add_header X-Frame-Options DENY;
# 允许单个域名iframe嵌套
add_header X-Frame-Options ALLOW-FROM http://whsir.com/;
# 允许多个域名iframe嵌套，注意这里是用逗号分隔
add_header X-Frame-Options "ALLOW-FROM http://whsir.com/,https://cacti.org.cn/";
```

详细地址：https://blog.whsir.com/post-3919.html





**XSS攻击**

XSS（Cross Site Script）即跨站脚本攻击，他将恶意脚本注入到目标网页中，用户在访问该页面时，有可能造成信息泄露、用户行为被劫持、感染并传播糯虫病毒等危害。攻击方式有**反射型**和**存储型**两种。反射型即用户提交的XSS代码出现在URL中，服务器解析后响应给浏览器，浏览器解析执行了XSS代码，这个过程想一次反射，所以称为反射型。存储型与反射型的区别在于存储型XSS代码会存储到服务器端（数据库，内存，文件系统等），因此造成的危害更大。

常见的XSS攻击场景：评论区，任何允许用户发布在网站上的内容。

防范方法：

1. 不要任性任何用户输入。
2. 对用户的输入进行编码（如HTML实体），过滤（如删除脚本代码）和校正（如DOM Parse）。

3. 另外在合适的情况下，可以为Cookie添加 HttpOnly 标记，使得客户端不能通过JavaScript读取 Cookie 信息，进一步保护用户账号（通常XSS攻击者都是为了获取用户的Cookie）。





**CSRF攻击**

CSRF攻击（Cross Site Request Forgery）即跨站请求伪造，攻击者伪装成正常用户对服务器发起请求，让服务器执行某些操作。例如用户正常登陆A网站，在未退出的情况下访问了攻击者事先准备好的B网站，此时攻击者就有可能获取到正常用户的 Cookie，然后攻击者就能伪装成该用户，与服务器进行通信。

防范方法：

1. 交互验证（让用户与网站进行交互才能完成请求，如在表单中添加验证码）
2. Refer验证（检查请求是否来自合法的源）
3. Token验证（在每个请求中添加一个Token参数，这是一个随机数，可以在进入页面时生成，然后保存在Session中，服务器在接收到Token参数时进行校验，只有当校验成功时才执行后面的操作）