---
title: Nodejs中进行DOM操作
description: '-'
tags:
  - WEB开发
  - Web后端
  - NodeJs
abbrlink: 7547a1b5
date: 2021-03-29 23:03:42
---



## 正文

**安装**

```bash
npm install cheerio 
或
yarn add cheerio
```

**使用**

```javascript
const cheerio = require('cheerio');

$ = cheerio.load('<h2 class="title">Hello world</h2>');
$('h2.title').text('Hello there!');
$('h2').addClass('welcome');
$.html();
//=> <h2 class="title welcome">Hello there!</h2>
```



## 参考

https://www.jb51.net/article/61133.htm