---
title: object不兼容处理
description: '-'
tags:
  - WEB开发
  - Web前端
  - 兼容问题
abbrlink: 8fee1a1e
date: 2021-03-29 22:03:17
---



## 前言

事情是这样子的，网站里面需要嵌入PDF文件预览，通常，可以借助`Flash`或者[PDF.js](https://github.com/mozilla/pdf.js/)等插件，但是更方便的办法是直接使用html的`<object>`标签或者`<embed>`标签。



这里，我使用了object标签, chrome和firefox一看没有问题，上线！！！

但是随后使用ie11看就出现问题了，PDF显示区域一片空白。说好的“IE浏览器对object标签有更好的兼容性，而非IE浏览器对embed标签则有更好的兼容性”呢![1612417142188](http://blog.cdn.ionluo.cn/blog/1612417142188.png)

![1612417042967](http://blog.cdn.ionluo.cn/blog/1612417042967.png)





因此，推荐以下写法同时使用`<object>`和`<embed>`标签以更好地兼容不同的浏览器：

```html
<object data="http://example.com/yourpdf.pdf" type="application/pdf" width="95%" height="700px">
    <embed src="http://example.com/yourpdf.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="http://example.com/yourpdf.pdf">Download PDF</a>.</p>
    </embed>
</object>
```





![1612417285315](http://blog.cdn.ionluo.cn/blog/1612417285315.png)



成功！





另外，这里需要切换显示pdf，一开始是使用的`Angularjs`的数据绑定PDF_URL变量，做到变量变化的时候重新加载新的pdf。在其他浏览器可以，但是IE下貌似object的data只能指定一次。 使用 `ng-attr-data` 在其他浏览器正常，但是IE下就还是无法绑定。因此，建议为了兼容，如果变量变化的情况不多的时候，一起渲染出来，通过 `ng-show` 来控制显示/隐藏。

