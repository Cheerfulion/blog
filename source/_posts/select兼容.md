---
title: select兼容
description: '-'
tags:
  - WEB开发
  - Web前端
  - 兼容问题
abbrlink: d7d51091
date: 2021-03-29 22:03:33
---



## 前言

一直认为IE11应该和现代浏览器比较接近了，今天打开网站一看，搜索的select默认样式好丑，于是想办法修改他的默认样式。

![1612250611152](http://blog.cdn.ionluo.cn/blog/1612250611152.png)





## 方法1：使用boostrap插件[Collapse](https://v3.bootcss.com/javascript/#collapse)等方式

该方式的缺点就是不是表单无法不通过js提交。但是样式兼容性最好。



## 方法2：[CSS重置默认样式](http://ourjs.com/detail/551b9b0529c8d81960000007)

原理是将浏览器默认的下拉框样式清除，然后应用上自己的，再附一张向右对齐小箭头的图片即可。（自测IE11无效，但是可以用在火狐浏览器和谷歌浏览器的样式兼容）

```css
select {
  /*Chrome和Firefox里面的边框是不一样的，所以复写了一下*/
  border: solid 1px #000;

  /*很关键：将默认的select选择框样式清除*/
  appearance:none;
  -moz-appearance:none;
  -webkit-appearance:none;

  /*在选择框的最右侧中间显示小箭头图片*/
  background: url("https://raw.githubusercontent.com/ourjs/static/gh-pages/2015/arrow.png") no-repeat scroll right center transparent;


  /*为下拉小箭头留出一点位置，避免被文字覆盖*/
  padding-right: 14px;
}


/*清除ie的默认选择框样式清除，隐藏下拉箭头*/
select::-ms-expand { display: none; }
```





IE并不支持 `appearance:none` 这个CSS属性，所以用overflow的hidden隐藏的方式去实现。我们需要为其添加一个父容器，容器是用来覆盖小箭头的，然后为select添加一个向右的小偏移或者宽度大于父级元素。设置父级的CSS属性为超出部分不可见，即可覆盖即小箭头。然后再为父级容器添加背景图片即可。overflow: hidden并不能隐藏下拉框的显示。

```html
<div id="parent">
  <select>
      <option>what</option>
      <option>the</option>
      <option>hell</option>
  </select>
</div>
```

```css
 #parent {
    background: url('yourimage') no-repeat;
    width: 100px;
    height: 30px;
    overflow: hidden;
}

#parent select {
    background: transparent;
    border: none;
    padding-left: 10px;
    width: 120px;
    height: 100%;
}
```

