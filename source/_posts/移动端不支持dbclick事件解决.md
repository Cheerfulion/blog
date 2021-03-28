---
title: 移动端不支持dbclick事件解决
description: 暂无描述！
tags:
  - WEB开发
  - Web前端
  - 兼容问题
abbrlink: 8e4c0560
date: 2021-02-12 13:31:15
---



## 前言

今天在做手机端双击全屏预览图片时候，发现无法触发dbclick事件，百度发现是移动端不支持dbclick，于是……

```javascript
//手机版图片双击出现弹窗,用于放大图片
angular.element('.manage-custom-content img').bind('dblclick', function (e) {
    $rootScope.openModal({
        template: 'web-modal-imgviewr',
        windowClass: 'modal-imgviewr',
        data: {src: e.currentTarget.src},
    });
});
```



## 正文

主要有两种方法，一是自定义事件dbclick，二是绑定click事件，当两次触发事件短的时候执行双击行为。

