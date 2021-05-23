---
title: 【前端】手机端预览pdf
date: 2021-05-23 23:35:18
tags:
	- 前端
	- 业务
---

 

原文链接：https://blog.csdn.net/u010476739/article/details/106025272



**环境：**

- pdfjs-2.4.456
- 华为nova2s
- 时间：2020-05-09

**问题来源：**
系统中需要提供pdf预览的功能，在pc端可以使用浏览器直接打开，但在手机端浏览器不能直接打开、微信也不能直接打开！！！

**pdf.js:**
官网地址：https://mozilla.github.io/pdf.js/
github地址：https://github.com/mozilla/pdf.js
使用这个脚本可以在PC端和手机端打开pdf，这里只用在手机端即可。

**使用步骤：**

- 第一步：下载pdfjs包
  ![在这里插入图片描述](https://www.pianshen.com/images/508/43d695d0bcef105353fef83811391634.png)
- 第二步：解压后将它放在web服务器上，浏览器直接打开即可![在这里插入图片描述](https://www.pianshen.com/images/494/56eb5884ffee596768b761e081d2c5fe.png)
- 第三步：浏览器打开看效果
  ![在这里插入图片描述](https://www.pianshen.com/images/366/42a7c77e85db8657f1b99a95dbeb008e.png)
  **预览同一网站的其他pdf：**
  将pdf文件放到同一个网站下，浏览器打开看效果：
  ![在这里插入图片描述](https://www.pianshen.com/images/219/cf7aaa0ac9f199f28370a18d2c078883.png)
  **预览其他网站的pdf：**
  这是个跨域问题，直接修改view.js:
  ![在这里插入图片描述](https://www.pianshen.com/images/676/8d1e978ffb52438ac253dec8c01e0f5c.png)
  然后，浏览器查看如下：
  ![在这里插入图片描述](https://www.pianshen.com/images/112/8cc6f09114fbfd92f9dc969e19dfb800.png)

**最后，手机端测试：**
注：微信、qq浏览器、uc浏览器都可以向pc端那样打开
![在这里插入图片描述](https://www.pianshen.com/images/287/bf7352a8d12b1363f92511b314db262f.png)