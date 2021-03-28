---
title: JSHint
description: 暂无描述！
tags:
  - WEB开发
  - Web前端
  - 代码规范
abbrlink: d5024ec3
date: 2021-01-20 00:15:24
---



# 我的配置`.jshintrc`

```json
{
  "curly": false, // 如果为真，JSHint会要求你在使用if和while等结构语句时加上{}来明确代码块。Javascript允许在if等结构语句体只有一句的情况下不加括号。不过这样做可能会让你的代码读起来有些晦涩。
  "eqeqeq": true, // 如果为真，JSHint会看你在代码中是否都用了===或者是!==，而不是使用==和!=。我们建议你在比较0，''(空字符)，undefined，null，false和true的时候使用===和!===。
  "eqnull": true, // 如果为真，JSHint会允许使用"== null"作比较。== null 通常用来判断一个变量是undefined或者是null（当时用==，null和undefined都会转化为false）。
  "forin": true, // 如果为真，那么，JSHint允许在for in 循环里面不出现hasOwnProperty，for in循环一般用来遍历一个对象的属性，这其中也包括他继承自原型链的属性，而hasOwnProperty可以来判断一个属性是否是对象本身的属性而不是继承得来的。
  "immed": false, // 如果为真，JSHint要求匿名函数的调用如下：(function(){//}());而不是(function(){//bla bla})();
  "passfail": true,  // 如果为真，JSHint会在发现首个错误后停止检查。
  // "maxerr": 5, // 设定错误的阈值，超过这个阈值jshint不再向下检查，提示错误太多。
  "newcap": true, // 如果为真，JSHint会要求每一个构造函数名都要大写字母开头。
  "noempty": true, // 如果为真，JSHint会禁止出现空的代码块（没有语句的代码块）。
  "nomen": true,  // 如果为真，JSHint会禁用下划线的变量名。很多人使用_name的方式来命名他们的变量，以说明这是一个私有变量，但实际上，并不是，下划线只是做了一个标识。如果要使用私有变量，可以使用闭包来实现。
  "quotmark": "true",  // 引号的使用规则, false : 不检查  true : 检查一致性（要么都是单引号，要么都是双引号）  single : 必须都是单引号  double : 必须都是双引号
  "undef": true, // 如果为真，JSHint会要求所有的非全局变量，在使用前都被声明。
  "unused": true, // 如果为真，禁止变量已经声明，但却不使用
  "white": true, // 如果为true，JSHint会依据严格的空白规范检查你的代码。
  "asi": true, // 如果是真，JSHint会无视没有加分号的行尾(默认，JSHint会要求你在每个语句后面加上分)
  "esversion": 5, // 先使语法中定义ECMAScript 5.1规范。这包括允许保留关键字作为对象属性。
  "globals": { // This declares to JSHint that $ is a global variable, and the false indicates that it should not be overridden.
    "$": false,
    "angular": false
  }
}
```

说明：https://www.cnblogs.com/rdst/p/4704151.html

https://www.jianshu.com/p/4cb23f9e19d3



上面定义JavaScript语法是ECMAScript 5.1规范，如果部分是用的ES6规范，在es6规范的文件加上`/* jshint esversion: 6 */`





**pycharm如果出现配置文件不生效的情况，打开“设置”， 搜索“JSHint”， 勾选“使用配置文件”，点击OK即可。**



![1606792603996](assets/JSHint/1606792603996.png)

![1606792627266](assets/JSHint/1606792627266.png)