---
title: js任意进制转换
description: '-'
tags:
  - WEB开发
  - Web前端
  - 随笔
abbrlink: 1a04c473
date: 2021-01-20 00:15:24
---



1. js可以很简单的做任意进制的转化，核心函数是全局函数`parseInt(str,radix)`与`Number.toString(radix)`;

   

2. `parseInt(str,radix)` 将字符串str按照radix进制编码方式转换为10进制返回，没有radix，默认为10； 此方法把任意进制字符串转为10进展返回。

   ```javascript
   console.log(parseInt('23',8));  //19
   ```

   

3. Number的一个方法：`toString(radix)`；返回表示该数字的指定进制形式的字符串。（把10进制的数据转为指定进制，并以字符串形式输出）；radix支持 [2, 36] 之间的整数。默认为10；

   ```javascript
   var x = 66;
   x.toString(16); //“42”
   ```



4. 所以问题是：把m进制的数字num转为n进制怎么做？

   ```javascript
   function conversion(num,m,n){
     var s = num+'';
     var result = parseInt(s,m).toString(n);
     return result;
   }
   ```

   以上函数把m进制的num转换为n进制。其中先用parseInt显示为10进制，再把10进制的数字用toString方法返回n进制的形式。