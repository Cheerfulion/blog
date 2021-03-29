---
title: JSHint遇到的一些问题
description: '-'
tags:
  - WEB开发
  - Web前端
  - 代码规范
abbrlink: 6d047e66
date: 2021-01-20 00:15:24
---



### pycharm JSHint:'let' is available in ES6(use 'esverion:6') or Mozilla JS extensions(use moz).(W104)

有两种解决办法：

1.在js顶部添加注释，`/* jshint esversion: 6 */`

2.在项目根目录下添加.jshintrc配置文件，在配置文件添加一段

```json
{
    "esversion": 6
}
```

