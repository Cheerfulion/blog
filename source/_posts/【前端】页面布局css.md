---
title: 页面布局css
description: '-'
tags:
  - WEB开发
  - Web前端
  - 随笔
abbrlink: 639a68cc
date: 2021-03-29 22:01:02
---



# 侧边栏布局

制作页面时经常遇到这种页面，这里提下实现这种侧边栏固定宽度，主题内容响应式的编码方案

![1605863518426](http://blog.cdn.ionluo.cn/blog/1605863518426.png)

1. 浮动

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <meta charset="utf-8">
       <title></title>
       <style type="text/css">
         * {
           margin: 0;
           padding: 0;
           box-sizing: border-box;
         }
   
         .aside {
           width: 200px;
           float: left;
           background-color: #1B6D85;
         }
   
         .main {
           width: 100%;
           padding-left: 210px;
           background-color: #C7254E;
           word-break: break-word;
         }
   
         .clearfix {
           clear: both;
         }
   
         
       </style>
     </head>
     <body>
       <div class="aside">
         xxxxx <br>
         xxxxx <br>
         xxxxx <br>
         xxxxx <br>
         xxxxx <br>
       </div>
       <div class="main">
         <div class="right">
         浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动
         <div class="clearfix"></div>
         浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动浮动
       </div>
       </div>
     </body>
   </html>
   ```

   > 注意，该方式有一个极大的缺陷，Main区域内不能使用清除浮动，否则会造成如下情况

   ![1605863854938](http://blog.cdn.ionluo.cn/blog/1605863854938.png)

2. table布局

3. calc

   > 支持程度不好

4. 定位

5. margin-left