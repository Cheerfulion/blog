---
title: 1.0 vuex
description: '-'
tags:
  - WEB开发
  - Web前端
  - vue
abbrlink: 2c46d5f5
date: 2021-03-29 22:16:10
---



# Vuex的使用

### Vuex的基本使用

![1597562090778](http://blog.cdn.ionluo.cn/blog/1597562090778.png)




![1597562137195](http://blog.cdn.ionluo.cn/blog/1597562137195.png)

### vuex的核心概念

![1597564451834](http://blog.cdn.ionluo.cn/blog/1597564451834.png)



#### State

![1597564556500](http://blog.cdn.ionluo.cn/blog/1597564556500.png)

![1597564898569](http://blog.cdn.ionluo.cn/blog/1597564898569.png)

#### Mutation

> Mutaion用于变更Store中的数据

![1597565350489](http://blog.cdn.ionluo.cn/blog/1597565350489.png)



> 注意: 上面的方式是错误的，vue不推荐在组件中直接修改全局store的数据。虽然可以执行，但是不利于后期项目的维护！！！

![1597565544755](http://blog.cdn.ionluo.cn/blog/1597565544755.png)

> 上面的写法才是合法的。mutations里面形参state就是vuex.store实例中的state对象（全局共享数据）



**Mutation传参**

![1597566082764](http://blog.cdn.ionluo.cn/blog/1597566082764.png)

![1597566873773](http://blog.cdn.ionluo.cn/blog/1597566873773.png)

#### Action

> Action用于处理异步任务

Mutation中直接用异步的方式修改Store中的数据Vuex是无法监控到的

![1597568125671](http://blog.cdn.ionluo.cn/blog/1597568125671.png)

![1597569450217](http://blog.cdn.ionluo.cn/blog/1597569450217.png)

#### Getter

![1597569650649](http://blog.cdn.ionluo.cn/blog/1597569650649.png)

![1597569961351](http://blog.cdn.ionluo.cn/blog/1597569961351.png)