---
title: 兼容html5和css3
description: '-'
tags:
  - WEB开发
  - Web前端
  - 兼容问题
abbrlink: 9690ea3b
date: 2021-03-29 22:06:58
---



# bootstrap模板为使IE6、7、8版本（IE9以下版本）浏览器兼容html5和css3

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Bootstrap的HTML标准模板</title>   
        <!-- Bootstrap -->
        <link href="/static/css/bootstrap.min.css" rel="stylesheet">
        <!--你自己的样式文件 -->
        <link href="/static/css/your-style.css" rel="stylesheet">        
        <!-- 以下两个插件用于在IE8以及以下版本浏览器支持HTML5元素和媒体查询，如果不需要用可以移除 -->
        <!--[if lt IE 9]>
        <script src="https://oss.maxcdn.com/libs/html5shiv/3.7.0/html5shiv.js"></script>
        <script src="https://oss.maxcdn.com/libs/respond.js/1.4.2/respond.min.js"></script>
        <![endif]-->
    </head>
    <body>
        <h1>Hello, world!</h1>

        <!-- 如果要使用Bootstrap的js插件，必须先调入jQuery -->
        <script src="http://libs.baidu.com/jquery/1.9.0/jquery.min.js"></script>
        <!-- 包括所有bootstrap的js插件或者可以根据需要使用的js插件调用　-->
        <script src="http://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script> 
    </body>
</html>
```





这里的css3也只是支持媒体查询，不支持css3的其他属性



如果不需要支持IE9以下浏览器，可以引入下面的代码：

[byebye-ie](http://blog.cdn.ionluo.cn/files/byebye-ie.zip)

```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Bootstrap的HTML标准模板</title>   
        <!-- Bootstrap -->
        <link href="/static/css/bootstrap.min.css" rel="stylesheet">
        <!--你自己的样式文件 -->
        <link href="/static/css/your-style.css" rel="stylesheet">        
        <!-- 以下两个插件用于在IE8以及以下版本浏览器支持HTML5元素和媒体查询，如果不需要用可以移除 -->
        <!--[if lt IE 9]>
        <script src="/static/js/byebye-ie/byebye-ie.zhCN.js"></script>
        <![endif]-->
    </head>
    <body>
        <h1>Hello, world!</h1>

        <!-- 如果要使用Bootstrap的js插件，必须先调入jQuery -->
        <script src="http://libs.baidu.com/jquery/1.9.0/jquery.min.js"></script>
        <!-- 包括所有bootstrap的js插件或者可以根据需要使用的js插件调用　-->
        <script src="http://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script> 
    </body>
</html>
```



