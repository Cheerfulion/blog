---
title: JavaScript中的注释
description: 暂无描述！
tags:
  - WEB开发
  - Web前端
  - 代码规范
abbrlink: f27d4e4e
date: 2021-03-06 12:57:00
---



## 文件注释

**@license文件注释 标注文件信息，介绍、版本信息、版权声明、开源协议、修改时间等说明。**

```javascript
/*
 @license 文件信息
 */
```

## 函数注释

@param 指定参数的名称。可以包含参数的数据类型，使用大括号括起来，和参数的描述。

```javascript
/*
 @param {string} a xxxxx
 */
```

**@returns 指定返回值**

```javascript
/*
 @retuns {string} a xxxxx
 */
```

## 其他注释

<table><thead><tr><th>标签</th><th>描述</th></tr></thead><tbody><tr><td></td><td></td></tr><tr><td>@module</td><td>当前文件模块，文件中的所有成员将被默认为属于此模块，除另外标明</td></tr><tr><td></td><td></td></tr><tr><td>@submodule</td><td>针对模块的划分，处于@module</td></tr><tr><td>@class</td><td>标示一个类或者一个函数</td></tr><tr><td>@constructor</td><td>对象字面量形式定义类时，可使用此标签标明其构造函数</td></tr><tr><td>@callback</td><td>标明是一个回调函数</td></tr><tr><td>@event</td><td>标明一个可触发的事件函数，一个典型的事件是由对象定义的一组属性</td></tr><tr><td>@constant</td><td>常量</td></tr><tr><td>@member/@var</td><td>一个基本数据类型的成员变量</td></tr><tr><td>@method</td><td>方法或函数</td></tr><tr><td>@param</td><td>方法参数及参数类型</td></tr><tr><td>@property</td><td>一个对象的属性</td></tr><tr><td>@readonly</td><td>只供阅读</td></tr><tr><td>@return</td><td>返回值、类型及描述</td></tr><tr><td>@type</td><td>代码变量的类型</td></tr><tr><td>@description</td><td>在注释开始描述可省略此标签</td></tr><tr><td>@enum</td><td>一个类中属性的类型相同时，使用此标签标明</td></tr><tr><td>@example</td><td>示例，代码可自动高亮</td></tr><tr><td>@exports</td><td>标识此对象将会被导出到外部调用</td></tr><tr><td>@ignore</td><td>忽略此注释块</td></tr><tr><td>@link</td><td>内联标签，创建一个链接</td></tr><tr><td>@name</td><td>指定一段代码的名称，强制使用此名称，而不是代码里的名称</td></tr><tr><td>@namespace</td><td>指定一个变量为命名空间变量</td></tr><tr><td>@static</td><td>描述一个不需实例即可使用的变量</td></tr><tr><td>@summary</td><td>描述信息的短叙述</td></tr><tr><td>@throws</td><td>方法将会出现的错误和异常</td></tr><tr><td>@todo</td><td>函数的功能或任务</td></tr><tr><td>@tutorial</td><td>一个指向向导教程的链接</td></tr></tbody></table>

