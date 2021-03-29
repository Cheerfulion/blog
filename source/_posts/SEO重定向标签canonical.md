---
title: SEO重定向标签canonical
description: '-'
tags:
  - WEB开发
  - Web前端
  - SEO
abbrlink: 35ff579f
date: 2021-03-29 22:11:29
---



## 什么是 canonical URL 标签？

canonical URL标签也叫规范网址，出现在你网页代码的<head>里。

以一灯博客为例，canonical URL标签的代码长下面这个样子。

```html
<link rel="canonical" href="https://www.1deng.me/" />
```



## 为什么要使用 canonical URL 标签？

首先，各大搜索引擎对网站SEO的考量包括了这个标签。

其次，很多网站SEO的问题就是不同的URL对应同一个页面内容。

以我们常见的产品页面的URL为例，大部分外贸独立站可能是下面这个样子。

https://www.yourdomain.com/products/

一旦你的产品多了有了分页就会自动生成一个新的URL。

https://www.yourdomain.com/products/page/2/

如果不做canonical URL标签优化，就搜索引擎机器人来看，上面两个URL的页面内容是一样的，所以机器人不知道到底要把哪个URL编入搜索结果里，也不知道哪个页面才是重要的。

无形间的重复内容，大大降低了页面在搜索引擎的重要性，页面权重也全都分散了。

虽然谷歌官方的说法是重复内容不直接影响SEO，但我可以很负责的告诉大家，大量的重复内容会分散你网站本该有的排名。有很多种办法可以处理重复内容，但最有效的还是用canonical URL标签把网址规范化。

否则谷歌会分不清楚你网站重复内容的页面哪个重要，哪个不重要。

而canonical URL标签的目的就是告诉谷歌把你重要的页面和其它页面区别对待，在搜索结果中只显示唯一的URL，规范URL的结构，让其它重复内容的URL指向最主要的那个URL。

**将链接权重传递到主页面。**



## 总结

canonical URL标签优化的定义，为什么要做，怎么做就介绍完了。

你可以安装yoast插件或者其它SEO插件来实现，但一些质量比较高的主题，比如avada，它的源代码本身就经过了canonical URL标签优化，这也是为什么它销量第一的其中一个原因。

最后，今天介绍的canonical URL的链接传递和301重定向是两回事，请不要把两者混淆了。

canonical URL是告诉搜索引擎你重复内容的URL哪一个才是最规范，最重要的，实际没有跳转。

canonical URL标签虽然能传递链接，告诉搜索引擎URL的唯一性和规范网址结构，但如果把它当301重定向用就有点过头了。

我见过有人想把某个页面的SEO排名顶上去，把首页的canonical URL都指向了想做排名的页面，搞到最后排名没上去，连首页都不收录了，哎......鸡飞蛋打想太多。



转自：https://www.1deng.me/canonical-url.html





我的页面：(这里用django模板引擎返回seo对象，包含`title`, `description`, `keyword`和`canonical`属性等，指定seo用到的一些属性)

```html
{% if seo.canonical %}
	<link rel="canonical" href="{{ seo.canonical }}"/>
{% else %}
	<link rel="canonical" href="{{ PROTOCOL }}{{ SITE_DOMAIN }}{{ request.path }}"/>
{% endif %}
```

