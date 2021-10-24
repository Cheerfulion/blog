---
title: 浏览器指纹
description: '-'
tags:
  - WEB开发
  - Web安全
abbrlink: 411492ba
date: 2021-03-28 18:33:54
---



## 前言

长期以来，人们一直认为IP地址和Cookies是用于跟踪在线人员的唯一可靠的数字指纹。**但是如今，当现代网络技术允许组织使用新的方法来识别和跟踪用户而又不必了解他们并且用户无法避免的时候（被动追踪），事情变得一发不可收拾。

`BrowserLeaks`与浏览隐私和Web浏览器指纹有关。在这里，您将找到Web技术安全测试工具的库，这些工具将向您显示可能泄露哪些类型的个人身份数据，以及如何保护自己免受此威胁。



https://browserleaks.com/ 

> 注：这个网站主要可以用来分析web应用可以获取的信息，如何规避这些信息的泄露。



**浏览器指纹**可以见这篇文章的介绍：

[可怕的“浏览器指纹”，让你在互联网上，无处可藏](https://xjjdog.blog.csdn.net/article/details/103573197)

简而言之，在Web应用中，你虽然没有注册账号，平台却为你分配了身份。而身份的特征是通过`cookie`，`ip`等小蛋糕的层面，或者更无解的js获取的`设备信息特征`，`canvas指纹`， `字体指纹`，`地理位置API`等方式。



下面转载一篇微信公众号上看到的一篇不错 的介绍文章：

> **原文地址：**https://mp.weixin.qq.com/s/iPmSVIbj24ZgDPLZ21WvPQ



## 浏览器指纹：原来我们一直被互联网巨头监视，隐私在网上裸奔、无处可藏

原创 夜尽天明 [全栈修炼](javascript:void(0);) *2020-03-16*

![图片](http://blog.cdn.ionluo.cn/blog/640-1609825675906.webp)

今天讲⼀些让您按捺不住和欲求不满的反浏览器追踪技术，揭开你是如果被互联网巨头监控的。

- 场景一：在⽹站上浏览了某个商品，了解了相关的商品信息，但并没有下单购买，甚⾄没有进⾏登录操作，过两天⽤同台电脑访问其他⽹站的时候却发现很多同类商品的⼴告。
- 场景二：在某博客中你有多个小号（水军），这些小号的存在就是为了刷某个帖子的热度或者进行舆论引导，又或者纯粹进行流量交易，即便你在切换账号的时候清空了cookie、本地缓存，重开路由器甚至使用 vpn 来进行操作，你觉得自己足够小心，并尽可能提高水军的真实性，但是管理人员可能还是知道这是同一个人在操作，从而被打击。

一般情况下，网站或者广告商都想要一种技术可以在网络上精确的定位到每一个个体，就算你没有账号，没有登录，也可以通过收集这些个体的数据，然后加以分析之后更加精确的去推送广告和其他的一些活动。

而这个技术就是浏览器指纹，这还是用前端技术来实现的。

![图片](http://blog.cdn.ionluo.cn/blog/640-1609825675913.webp)

## **定义**

游览器指纹，就像现实生活中人的指纹一样，特异地标记着每个上网用户。

浏览器指纹：是一种通过浏览器对网站可见的配置和设置信息来跟踪Web浏览器的方法，浏览器指纹就像我们人手上的指纹一样，具有个体辨识度，只不过现阶段浏览器指纹辨别的是浏览器。

人手上的指纹之所以具有唯一性，是因为每个指纹具有独特的纹路、这个纹路由凹凸的皮肤所形成。每个人指纹纹路的差异造就了其独一无二的特征。

那么浏览器指纹也是同理，获取浏览器具有辨识度的信息，进行一些计算得出一个值，那么这个值就是浏览器指纹。

辨识度的信息可以是UA、时区、地理位置或者是你使用的语言等等，你所选取的信息决定了浏览器指纹的准确性。

## **指纹技术历史**

1. 第 1 代：服务端在客户端设置标志

   第一代指纹追踪是 cookie 这类的服务端在客户端设置标志的追踪技术，evercookie 是 cookie 的加强版。

2. 第 2 代：单浏览器指纹

   第二代指纹追踪是设备指纹技术，发现 IP 背后的设备。

   通过 js 获取操作系统、分辨率、像素比等等一系列信息，传到后台计算，然后归并设备。

3. 第 2.5 代：跨浏览器指纹识别技术。

   跨浏览器之后，第二代技术中很重要的 canvas 指纹、浏览器插件指纹都变了，所以很难把跨浏览器指纹归并到同一设备上。

   因为设备指纹相同，很大概率上是同一台设备；但是，设备指纹不同时，不一定不是同一台设备。

4. 第三代：跨设备指纹

   第三代指纹追踪技术，则是发现设备后面的人。通过人的习惯、人的行为等等来对人进行归并，此项技术比较复杂。

5. 总 结

   第一代、第二代的指纹追踪技术是可以直接通过 js 收集信息的，第三代指纹追踪技术目前可看到的案例是 2017 年 RSA 创新沙盒的冠军 unifyid 技术，unifyid 在移动端安装软件、收集信息，不仅仅是通过 js。

   至于利用于 web 上，还任重而道远。

## **应用**

- 分析可能导致识别欺诈者和其他需要调查的可疑活动
- ⼴告营销机器获取您的数据，以便跟踪您的在线活动
- 银⾏使⽤此⽅法来识别潜在的欺诈案件

![图片](http://blog.cdn.ionluo.cn/blog/640-1609825675875.webp)

## **是什么让你暴露身份**

当你使⽤浏览器访问某个⽹站的时候，浏览器【必定会暴露】某些信息给这个⽹站。

有些是跟 HTTP 协议相关的。

只要你基于 HTTP 协议访问⽹站，浏览器就【必定】会传输这些信息给⽹站的服务器。

因此，Web ⽹站的服务器必定可以获取到跟你的浏览器相关的某些信息

```
{  "headers": {    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3",    "Accept-Encoding": "gzip, deflate, br",    "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8",    "Host": "httpbin.org",    "Sec-Fetch-Mode": "navigate",    "Sec-Fetch-Site": "none",    "Sec-Fetch-User": "?1",    "Upgrade-Insecure-Requests": "1",    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36"  }}
```

## **基本的浏览器指纹**

![图片](http://blog.cdn.ionluo.cn/blog/640-1609825675907.webp)



**指纹采集**

> 信息熵（entropy）是接收的每条消息中包含的信息的平均量，熵越高，则能传输越多的信息，熵越低，则意味着传输的信息越少。

浏览器指纹是由许多浏览器的特征信息综合起来的，其中特征值的信息熵也是不尽相同。

点击 这里 查看自己的浏览器指纹ID和基本信息。

![图片](http://blog.cdn.ionluo.cn/blog/640.jpg)

这个网站也可以查看你浏览器的指纹相关信息：https://amiunique.org/fp。

它可查看到哪些信息呢？如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icLlxzKkLGIee7pHUQmp3Lht8OibsIjWol9v3k0BdSpEgQ0A5wcqD4AUoGauz8ns5vXQRS7JSPrVeEuWraydMoMA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





而且浏览器指纹还有一个开源项目了，纯 JS 实现的，只有引用这个项目就可以获取浏览器的各种信息或者系统的配置了。

现代而灵活的浏览器指纹库：https://github.com/Valve/fingerprintjs2。

使用也很简单，如下：

**安装（Installation）**

- Bower: bower install fingerprintjs2
- NPM: npm install fingerprintjs2
- Yarn: yarn add fingerprintjs2

**使用（Usage）**

```
if (window.requestIdleCallback) {    requestIdleCallback(function () {        Fingerprint2.get(function (components) {          console.log(components) // an array of components: {key: ..., value: ...}        })    })} else {    setTimeout(function () {        Fingerprint2.get(function (components) {          console.log(components) // an array of components: {key: ..., value: ...}        })    }, 500)}
```

还有一个用纯JavaScript编写的设备信息和数字指纹的开源项目：https://github.com/jackspirou/clientjs。

## **总结**

科技公司通过大数据，会对你进行一个大体的画像，然后按照你的喜好推送信息。

- 比如一些精准的广告，刺激你荷尔蒙的小视频等。

- 就拿你在玩的抖音来说，你其实可以匿名使用，但是你爱抖胸妹子的喜好，不会因为重装抖音而消失，它已熟知了你的癖好。

- 这些收集你浏览器信息的动作，默默的在后台发生，用户根本毫无觉察。

- 你的每一次点击，都无情的出卖了你，这些信息会被综合分析，相关网站和部门，能够对你进行唯一性识别，进而锁定、追踪。

- 你虽然没有注册账号，平台却为你分配了身份。

- 这是识别方式，用于识别你这个个体，而收集的内容，可能更让人瞠目结舌，不要觉得垃圾数据多，存不下，行为数据比那些廉价的磁盘，值钱的多。

- 包括你的每一次点击，停留的时长，阅读、观看的位置，都在全方位的展示你的个体。

- 设备、IP、位置、操作习惯，都在不同的角度绘制你的指纹，让你在匿名的互联网上，无处可藏。

  

如果你没有足够专业的知识或者非常频繁更换浏览器信息的话，几乎 100% 可以通过浏览器指纹定位到一个用户，当然这也不见得全是坏事。

- 泄露的隐私非常片面，只能说泄露了用户部分浏览网页时的行为。
- 价值不够，用户行为并未将实际的账户或者具体的人对应起来，产生的价值有限。
- 有益利用，利用浏览器指纹可以隔离部分黑产用户，防止刷票或者部分恶意行为。



## 补充

### 基础指纹

　基本指纹是任何浏览器都具有的特征标识，比如硬件类型（Apple）、操作系统（Mac OS）、用户代理（User agent）、系统字体、语言、屏幕分辨率、浏览器插件 (Flash, Silverlight, Java, etc)、浏览器扩展、浏览器设置 (Do-Not-Track, etc)、时区差（Browser GMT Offset）等众多信息，这些指纹信息“类似”人类的身高、年龄等，有很大的冲突概率，只能作为辅助识别。

硬件类型：

操作系统：navigator.platform

用户代理（浏览器类型）：navigator.userAgent

浏览器版本号：navigator.appVersion

浏览器是否启动cookie：navigator.cookieEnabled

cpu等级：navigator.cpuClass

cpu虚拟核心数：navigator.hardwareConcurrency

系统语言：navigator.language

屏幕分辨率：window.screen

浏览器插件：navigator.plugins

时差区：new Date().getTimezoneOffset()  //-480 （负值是东区，480分钟为8小时，所以中国是东八区）

颜色质量：window.screen.colorDepth

```javascript
//判断是否IE
function isIe(){
  var i=navigator.userAgent.toLowerCase().indexOf("msie");
 return i>=0;
}
//判断是否firefox
function isFireFox(){
  var i=navigator.userAgent.toLowerCase().indexOf("firefox");
 return i>=0;
}
//拿到浏览器指纹的位置 
var options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0
};

function success(pos) {
  var crd = pos.coords;

  console.log('Your current position is:');
  console.log('Latitude : ' + crd.latitude);
  console.log('Longitude: ' + crd.longitude);
  console.log('More or less ' + crd.accuracy + ' meters.');
};

function error(err) {
  console.warn('ERROR(' + err.code + '): ' + err.message);
};

navigator.geolocation.getCurrentPosition(success, error, options);
```

### 高级指纹

基本指纹就像是人的外貌特征，外貌可以用男女、身高、体重区分，然而这些特征不能对某个人进行唯一性标识，仅使用基本指纹也无法对客户端进行唯一性判定，基于HTML5的诸多高级指纹对此提供了新思路。

（1）在cookie中种入UUID

　　下面是具体的实现方法

```
function rand(len) {
    var hex = "0123456789abcdef",
        str = "",
        index = 0;
    for (len = len || 32; len > index; index++) {
        str += hex.charAt(Math.ceil(1e8 * Math.random()) % hex.length);
    }
    return str;
}

var uuid = (new Date).getTime() + "_" + rand();
```

　　但是随着互联网对个人隐私的重视，cookie越来越不受待见，不少安全工具甚至是浏览器都开始允许或引导用户关闭Cookie功能。这就导致使用cookie的收效甚微。这时候，人们看到了Canvas指纹。

（2）Canvas指纹

　　从根本上来说，每一种浏览器都会使用不同的图像处理引擎，不同的导出选项，不同的压缩等级，所以每一台电脑绘制出的图形都会有些许不同，这些图案可以被用来给用户设备分配特定编号（指纹），也就是说可以用来识别不同用户。

```
function bin2hex(s) {
  var i, l, o = '',
    n;

  s += '';

  for (i = 0, l = s.length; i < l; i++) {
    n = s.charCodeAt(i)
      .toString(16);
    o += n.length < 2 ? '0' + n : n;
  }

  return o;
}

function getUUID(domain) {
    var canvas = document.createElement('canvas');
    var ctx = canvas.getContext("2d");
    var txt = domain;
    ctx.textBaseline = "top";
    ctx.font = "14px 'Arial'";
    ctx.textBaseline = "tencent";
    ctx.fillStyle = "#f60";
    ctx.fillRect(125,1,62,20);
    ctx.fillStyle = "#069";
    ctx.fillText(txt, 2, 15);
    ctx.fillStyle = "rgba(102, 204, 0, 0.7)";
    ctx.fillText(txt, 4, 17);

    var b64 = canvas.toDataURL().replace("data:image/png;base64,","");
    var bin = atob(b64);
    var crc = bin2hex(bin.slice(-16,-12));
    return crc;
}

console.log(getUUID("https://www.baidu.com/"))
```

　　测试结果表明，同一浏览器访问该域时生成的CRC校验码总是不变。可以简单理解为同样的HTML Canvas元素绘制操作，在不同的操作系统不同的浏览器上，产生的图片内容其实是不完全相同的。出现这种情况可能是有几个原因：

1. 在图片格式上，不同web浏览器使用了不同的图形处理引擎、不同的图片导出选项、不同的默认压缩级别等。
2. 在像素级别来看，操作系统各自使用了不同的设置和算法来进行抗锯齿和子像素渲染操作。
3. 即使是相同的绘图操作，最终产生的图片数据在hash层面上依然是不同的。

（3）硬件指纹

　　硬件指纹主要通过检测硬件模块获取信息，作为对基于软件的指纹的补充，主要的硬件模块有：GPU’s clock frequency、Camera、Speakers/Microphone、Motion sensors、GPS、Battery等。

> 转自：https://www.cnblogs.com/leijing0607/p/8044218.html





## 浏览器指纹登录请看：https://juejin.cn/post/6935811262752227335