---
title: Less的积累
description: '-'
tags:
  - WEB开发
  - Web前端
  - Css预处理
  - Less
abbrlink: c8618756
date: 2021-01-20 00:15:24
---



这里是平时写代码的一些小经验积累。



### 1. 媒体查询

像平时使用Bootstrap等使用媒体查询的UI框架时，经常需要定义不同的媒体设备宽度监测，如果写太多 @media (max-width: 768px) 的确是一件很烦人的事情，所以可以先把这些存成变量来使用就好了。

```less
@min768: ~"(min-width: 768px)";
@min992: ~"(min-width: 992px)";
@min1280: ~"(min-width: 1280px)";

@max768: ~"(max-width: 768px)";
@max992: ~"(max-width: 992px)";
@max1280: ~"(max-width: 1280px)";

.aside {
    @media @max768 {
        display: none;
    }
    
    @media @max992 {
        width: 200px;
    }
    
    @media @max1280 {
        width: 360px;
    }
}
```

