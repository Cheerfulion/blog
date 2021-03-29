---
title: js导入Excel数据
description: '-'
tags:
  - WEB开发
  - Web前端
  - 随笔
abbrlink: 238fbc80
date: 2021-03-29 22:00:16
---





## 读取数据

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
      <input type="file">
      <script src="../asset/js/xlsx.core.min.js"></script>
      <script>
        var file = document.getElementsByTagName('input')[0];
        file.onchange = function(changeEvent) {
            console.log("加载文件中……");
            var reader = new FileReader();
            // changeEvent.target.files[0]
            reader.readAsBinaryString(file.files[0]);
            reader.onload = function(e) {
                var bstr = e.target.result;
                var workbook = XLSX.read(e.target.result, {type: 'binary'}); // 以二进制流方式读取得到整份excel表格对象

                var data = []; // 存储获取到的数据

                // 遍历每张表读取
                for (var sheet in workbook.Sheets) {
                    if (workbook.Sheets.hasOwnProperty(sheet)) {
                        const fromTo = workbook.Sheets[sheet]['!ref'];
                        // console.log("fromTo", fromTo);
                        data = data.concat(XLSX.utils.sheet_to_json(workbook.Sheets[sheet]));
                    }
                }
                console.log('数据读取成功', JSON.parse(JSON.stringify(data)));
            }
        }
      </script>
  </body>
</html>
```





## 问题记录

2020-11-19 

### 关于js导入Excel时，Excel的(年/月/日)日期是五位数字的问题

> 转自：https://blog.csdn.net/qq_40662765/article/details/109326067

####  前言

近期实现了一个前端导入Excel的需求，然后问题就来了，Excel传来的日期是一串数字！！！
就比如：2020年10月28日是44132。根据度娘得知Excel存储的日期是从1900年1月1日开始按天数来计算的，也就是说1900年1月1日![20201028100834973](http://blog.cdn.ionluo.cn/blog/20201028100834973.png)在Excel中是1。

![20201028101225317](http://blog.cdn.ionluo.cn/blog/20201028101225317.png)



#### 思路

我一开始的想法是：

因为时间戳是从1970年1月1日算起的（时间戳为0的时候是1970年1月1日）也就是说`new Date(0).toLocaleDateString('zh')`的值是`1970/1/1`。
而1970年1月1日这一天在Excel中是`25569`，那就令从Excel中获取到的值减去25569，然后再乘以`24*60*60*1000`获取到这一天的毫秒数，再`new Date(这个毫秒数)`就能得到转换后的日期了。

不过刚刚在写这篇博客之前再次测试发现这样写有漏洞…

就是当Excel的值小于61的时候转换的时间跟正常时间相差一天
![20201028143240571](http://blog.cdn.ionluo.cn/blog/20201028143240571.png)

问题所在的原因：

在Excel中：

- 1是1900年01月01日
- 59是1900年02月28日
- **60是1900年02月29日** 1900年是平年，没有这一天！！！错误的原因就是这个
- 61是1900年03月01日
- 62是1900年03月02日

**解决办法**：既然60之前相差一天 那就做个判断 <60的时候 多减去一天

#### 最终代码

经过解决和我自己测试后的最终代码如下（仅适用于Excel中`年月日`日期的转换，`不包含时分秒`）：

```javascript
/**
 * 格式化Excel表格中存储的时间
 * @param {Number} num:	excel存储的数字
 * @param {String} format: 年月日的间隔符，默认为'-'
 */
function formatExcelTime(num, format = '-') {
	num = Number(num);	// 强制类型转化，以防传来的值是字符串
	let millisecond = 0;	// 转化后的毫秒数
	if (num > 60) {
		millisecond = (num - 25569) * 60 * 60 * 24 * 1000;
	} else {
		millisecond = (num - 25568) * 60 * 60 * 24 * 1000;
	}
	let date = new Date(millisecond);	// 根据转化后的毫秒数获取对应的时间
	let yy = date.getFullYear();
	let mm = date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1;
	let dd = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
	return yy + format + mm + format + dd;	// 返回格式化后的日期
}
```