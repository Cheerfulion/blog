---
title: NAS最强攻略
description: 暂无描述！
tags:
  - NAS
abbrlink: 573fa926
date: 2021-02-12 13:31:14
---



# NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！

原文链接：https://post.smzdm.com/p/ag87w953/

**创作立场声明：**熬了几个通宵，折腾了数周，终于打造出我心目中完美的all in one设备！本文超过一万字，150+张图，建议先收藏，再观看，绝对干货！切记，不喜勿喷！

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed15e0f535735297.jpg_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_2/)

大家好，俺又来了！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/33.png)

今天又是NAS相关的文章，绝对干货！如果您对NAS一窍不通，那么这篇文章，建议先收藏，在站内搜索下NAS相关的文章，再考虑要不要继续看下去，本文适合有一定基础的朋友观看！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/36.png)

最近组了一台all in one的NAS，网络组网图如下：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/39.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13fd8bfd884642.jpg_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_3/)

我的硬件用的i5-8400处理器，Z370主板，安装了万兆网卡，2条NVME固态做缓存，虚拟了软路由、Win10，黑群晖系统，可以说非常强大，硬件的组装过程，看这篇文章：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/60.png)

[![img](asset/新建文本文档/5ecd3af80067b9728.jpg_a200.jpg)](https://post.smzdm.com/p/a8306npl)[**我爱捡垃圾 篇十六：组装一台高性能All in one NAS，i5处理器+z370主板，万兆+软路由+win10！**](https://post.smzdm.com/p/a8306npl)想攒一台电竞主机、家用主机、酷炫主机无从下手？想省钱又怕性能不达标？值得买帮你打造定制化DIY装机工具，自助全网比价装机，提供最适合的搭配方案，解决各种攒机场景下难题。>快快使用戳这里点击这里查看详情[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*1k*评论*489*收藏*3k[查看详情](https://post.smzdm.com/p/a8306npl)

对于上篇文章的配置，估计大多家庭都用不上双M.2固态做缓存，也用不上万兆网卡，所以我又弄了一套减配方案：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/35.png)

主要把CPU更换成i3-8100、主板更换成 H310M主板，这些都非常好买，而且质量几乎都不赖。

CPU散热器和内存在618也会便宜不少，这样这么一套就很值了，大家不要无脑抄作业，可以根据自身需求再来改变方案：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/38.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed347b783ab24921.jpg_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_4/)

这次在UNRAID系统上，实现 all in one 功能，还是很不容易的，过程巨复杂。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/64.png)

现在已经稳定运行30天，看视频、转码、挂pt、传输文件、下载、备份相册 都没有任何问题，我[小站的导航页](http://www.junwen.bid:1800/)也安装在unRAID上了：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed3dd703ddd4529.jpg_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_5/)

目前这台NAS有安全存储，软路由，黑群晖，大容量空间，WIN10系统，影音转码，以及万兆内网：

可以说，是我目前心目中最完美的NAS状态了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed157e12fe649032.gif_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_6/)

我在每实现一个功能的时候，进行了笔记，现在将这些笔记，整理成这篇文章，分享给大家。

我争取做到，手把手教给大家，从零打造一台all in one系统！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)



也许有朋友认为all in one不好，一旦出问题，全家都没有网络，而且维护起来很麻烦！

**我只能说，确实是这样子的！**最开始弄all in one，确实容易出故障，但是**只要弄好后，就很稳了！**![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

如果您是一个怕麻烦的人，还是建议您每个设备分开！

其实很多出故障的设备，**大多都是配置不够**，或者**软件搭建**方面出了问题！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/56.png)

目前我**非常有信心** 我能做到稳定，我确定我可以维护好这台all in one设备了。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/118.png)

这篇文章，其实可以拆成好几篇文章，内容和信息量都巨大，不过如果拆分多篇文章，大家观看起来就容易前后不着村，所以合并成一篇文章。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/36.png)

本文**字数超过一万字**，希望大家先收藏，然后慢慢跟着文章进行操作！





在开始正文之前，回答上面一篇文章的几个小问题：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

1、首先是网卡的选择，很多人问这个网卡，能不能拆分直通，我这次买的是pcie*1 转4网卡，可以拆分的，文章后面也有分享软路由的安装方法，我的网卡是下面这张：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed146037fe4c66.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_7/)

但是我更推荐大家口碑更好的I350T4网卡：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

天猫精选[![img](asset/新建文本文档/5ed1464b313677962.jpg_a200.jpg)](https://go.smzdm.com/e31b0dd473e0472c/ca_bb_yc_163_ag87w953_10869_1735_173_0)[**DIEWUI350四口千兆网卡PCI-E服务器4口千兆网卡Inteli350t4多口网卡汇聚软路由4口千兆网卡**](https://go.smzdm.com/e31b0dd473e0472c/ca_bb_yc_163_ag87w953_10869_1735_173_0)200元[去购买](https://go.smzdm.com/e31b0dd473e0472c/ca_bb_yc_163_ag87w953_10869_1735_173_0)

2、然后就是U盘的选择，UNRAID系统，是安装在U盘上面的！很多人担心会坏，其实这个系统非常的轻量，U盘买个大厂的就好一些，USB2.0 、 USB3.0 都可以用的，只要做好备份，就不用担心出问题：

我是推荐体积小一点的U盘，比如闪迪酷豆，我就是用的这个：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

天猫精选[![img](asset/新建文本文档/5ed146a369e688126.jpg_a200.jpg)](https://go.smzdm.com/e9c40d86d5b613ad/ca_bb_yc_163_ag87w953_10869_1735_173_0)[**闪迪酷豆USB闪存盘CZ3316G小巧迷你车载U盘优盘**](https://go.smzdm.com/e9c40d86d5b613ad/ca_bb_yc_163_ag87w953_10869_1735_173_0)34.9元[去购买](https://go.smzdm.com/e9c40d86d5b613ad/ca_bb_yc_163_ag87w953_10869_1735_173_0)

3、关于机箱，特别提醒，夏天到了，机箱别选择闷罐的，一定要做到散热更好，不然因为散热不足导致的数据损坏，就太可惜了，我都是过来人，不然也不会有这篇文章的：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/36.png)

我就是因为散热没做好处理，导致数据损坏，所以这次非常看重散热性能！机箱选择我这样又便宜，散热又好的，孔比较多，可以放多个散热风扇的，是最好的：

天猫精选[![img](asset/新建文本文档/5fa62ceeeef352491.jpg_a200.jpg)](https://go.smzdm.com/a2492433a9252e09/ca_bb_yc_163_ag87w953_10869_1735_173_0)[**Thermaltake Tt 开拓者 M3 机箱 黑色**](https://go.smzdm.com/a2492433a9252e09/ca_bb_yc_163_ag87w953_10869_1735_173_0)119元[前板镂空](https://www.smzdm.com/p/28969660/)[支持ITX/MATX主板](https://www.smzdm.com/p/28969660/)[去购买](https://go.smzdm.com/a2492433a9252e09/ca_bb_yc_163_ag87w953_10869_1735_173_0)

4、最后一个，关于硬盘支架的问题，上面这个机箱，虽然只有2个3.5寸硬盘位，但是可以用硬盘支架，将硬盘放到前面的区域，配合无痕胶可以完美固定：

这个支架是立人的硬盘支架，这里放上链接：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/54.png)

天猫精选[![img](asset/新建文本文档/5dd3837ba11d84363.jpg_a200.jpg)](https://go.smzdm.com/02ce6d9ae125b960/ca_bb_yc_163_ag87w953_10869_1735_173_0)[**E.mini 立人 硬盘架E-D3 E-D5 E-D5S E-M3 E-M5机箱专用**](https://go.smzdm.com/02ce6d9ae125b960/ca_bb_yc_163_ag87w953_10869_1735_173_0)25.90元起实时价格15小时前已更新[去购买](https://go.smzdm.com/02ce6d9ae125b960/ca_bb_yc_163_ag87w953_10869_1735_173_0)

无痕胶，粘的可牢固了：

天猫精选[![img](asset/新建文本文档/5e8d396cb81268207.jpg_a200.jpg)](https://go.smzdm.com/17a2c5b04ca231c0/ca_bb_yc_163_ag87w953_10869_1735_173_0)[**奔亿达 无痕双面胶 30mm\*1米 1卷装**](https://go.smzdm.com/17a2c5b04ca231c0/ca_bb_yc_163_ag87w953_10869_1735_173_0)2.8元包邮（需用券）[淘宝好评率96%](https://www.smzdm.com/p/29792748/)[撕下无痕](https://www.smzdm.com/p/29792748/)[去购买](https://go.smzdm.com/17a2c5b04ca231c0/ca_bb_yc_163_ag87w953_10869_1735_173_0)

优惠

满4元减2元

扫码领取

[查看更多商城](https://wiki.smzdm.com/p/pn01g30/)

好了，补充完毕！接下来开始正片内容！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/68.png)





## 一、unRAID系统安装篇

因特殊原因，这里无法展示unRAID系统的安装过程，请大家通过这篇文章了解，我这里就不过多写了，不然文章无法过审：

我安装的是6.8.1 的版本！

切记，对UNRID是新手的朋友，请一定要看下面这篇文章：

[![img](asset/新建文本文档/5df90080d850f4366.jpg_a200.jpg)](https://post.smzdm.com/p/aoow5ml7)[**NAS教程：手把手教您 3分钟安装UNRAID系统 并设置硬盘共享文件 Docker容器APP**](https://post.smzdm.com/p/aoow5ml7)前言大家好，俺又来了！久等了！UNRAID是一款类似NAS的操作系统，和PVE、ESXI很像，但又很不同。在我使用了大半个月后，已经完美搭建出自己的家用NAS服务器，我觉得非常好用，所以推荐给大家。老规矩，废话少说，本文都是干货，欢迎大家提前点赞、点收藏，谢谢大家的支持！下面这篇关于UNRAID的介[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*796*评论*499*收藏*4k[查看详情](https://post.smzdm.com/p/aoow5ml7)

## 二、系统初始化篇

在完成安装系统后，而且有熟悉过UNRAID系统，就可以开始：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/121.png)

1、最开始我是添加一个120G的固态硬盘做阵列，需要格式化xfs格式，上面安装的文章都有详写：

一开始是打算只给一个固态给UNRAID，其它的磁盘用来直通，后来我改了，大家先跟着我的继续看：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d861948f1470.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_8/)

2、为了方便smb连接UNRAID稳定使用，需要新建一个账户，我这里建立的admin的账户：

并且为了安全，还修改一下root账号的密码：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8619d303884.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_9/)

3、修改UNRAID的时区信息：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

点击按钮：SETTINGS——Date and Time

修改时区信息如下：

ntp1.aliyun.com

s1b.time.edu.cn

ntp3.aliyun.com

hk.pool.ntp.org

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8628fff5336.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_10/)

4、设置开机自动启动阵列：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

点击按钮：SETTINGS——Disk Settings 改成YES

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d86b82ed754.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_11/)

5、设置APPS市场，用新的链接：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

点击按钮：PLUGINS，填下面的链接，INSTALL即可：

如果APPS安装失效，有可能是网络不通，过一阵子再试试，先安装别的！

> https://gitee.com/BlueBuger/community.applications/raw/master/plugins/community.applications.plg

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d86b217f9851.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_12/)

6、如果自己没有梯子，或者打开APPS很慢，建议设置Docker加速！

参考教程：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![img](asset/新建文本文档/5dfce95b969635887.jpg_a200.jpg)](https://post.smzdm.com/p/amm5lzvp)[**私人云搭建 篇七：和阿文菌大佬一起换了unraid初体验，补充unraid设置细节！群晖不香咯~**](https://post.smzdm.com/p/amm5lzvp)Hello，从几个月前就在琢磨群晖的jellyfin硬解，直到最近终于用unraid成功代替群晖达成unraid硬解。jellyfin终于完成了硬解，了结群里好多硬解的梦想（我们真的是尝试了好几个月又不想用别的系统在群晖吊死了好几个月直到unraid被UP主挖掘）主要是大家都喜欢玩docker应用p[劉壮实](https://zhiyou.smzdm.com/member/4393537765/)|*赞*121*评论*148*收藏*1k[查看详情](https://post.smzdm.com/p/amm5lzvp)

7、在SHARES目录下，点击ADD，新建一个共享文件夹，我建立的名叫NAS的文件夹：

并且设置一下 私有化，设置 admin用户 读写权限：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/109.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d86b1fbe23.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_13/)

8、在电脑文件夹的地址栏，输入UNRAID的IP地址，映射一下这个文件夹：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/68.png)

以后就可以当本地文件夹访问了！

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d86e195c2141.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_14/)

9、以上这些都是最基础的初始化方法，之前也写过保姆级教程，也可以搭配着观看：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/99.png)

[![img](https://qna.smzdm.com/201912/18/5df90080d850f4366.jpg_a200.jpg)](https://post.smzdm.com/p/aoow5ml7)[**NAS教程：手把手教您 3分钟安装UNRAID系统 并设置硬盘共享文件 Docker容器APP**](https://post.smzdm.com/p/aoow5ml7)前言大家好，俺又来了！久等了！UNRAID是一款类似NAS的操作系统，和PVE、ESXI很像，但又很不同。在我使用了大半个月后，已经完美搭建出自己的家用NAS服务器，我觉得非常好用，所以推荐给大家。老规矩，废话少说，本文都是干货，欢迎大家提前点赞、点收藏，谢谢大家的支持！下面这篇关于UNRAID的介[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*796*评论*499*收藏*4k[查看详情](https://post.smzdm.com/p/aoow5ml7)

10、拷贝速度测试，110MB/s，千兆可以跑满，处理器占用比例并不大：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/39.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d86c72377332.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_15/)

11、安装挂载插件：Unassigned Devices

直接是APPS里面搜索这个插件，点击安装即可：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

这个插件可以挂载局域网的其它NAS的磁盘，到后续我们虚拟机装了群晖NAS系统后，需要用这个插件进行挂载：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d86daca25821.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_16/)

12、添加集成显卡驱动到go文件：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

集成显卡驱动的修改，参考我Jellyfin的教程：

需要用影音转码功能的朋友可以弄这个，否则可以跳过：

[![img](asset/新建文本文档/5dfa5cf35a4285097.jpg_a200.jpg)](https://post.smzdm.com/p/a25gpmpn)[**UNRAID教程：3分钟 用安装Jellyfin 开启硬件加速转码 解码4K 打造最强家庭影院**](https://post.smzdm.com/p/a25gpmpn)前言大家好，俺又来了！这是我写的关于UNRAID第三篇文章了，依旧是保姆级文章，搭配前面两篇食用，效果会更佳。最近站里分享UNRAID的文章越来越多，这是一件非常好的事情。玩的人越多，就越多精彩的内容，已经看到不少干货文章出现了！今天教给大家如何用UNRAID开启集成显卡的硬件加速功能，让Jelly[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*486*评论*364*收藏*3k[查看详情](https://post.smzdm.com/p/a25gpmpn)



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d87335af953.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_17/)

13、安装Dynamix S3 Sleep 休眠插件：

直接在APPS搜索这个插件，安装即可：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/34.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d87397011064.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_18/)

安装上面的休眠插件以后，在这里就有一个休眠按钮，点一下就进入休眠了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/60.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8743c3d953.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_19/)

休眠插件的很多设置可以参考下面[这个视频](https://www.youtube.com/watch?v=1TTsV9nfazk&list=PL6MCtOroZNDCXhQQWrVjbPWO-45qV7SOF&index=2)进行设置，大概6-7分钟左右的地方：

14、安装CPU温度显示插件：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

参考教程：

[![img](asset/新建文本文档/5ecb4ec6d64339286.jpg_a200.jpg)](https://post.smzdm.com/p/aekeqz2q)[**unraid折腾笔记 篇十二：unraid实现温度检测，手把手教你安装温度检测插件**](https://post.smzdm.com/p/aekeqz2q)写在前面前段时间给大家推荐了unraid必装的13款插件，没看过的小伙伴可以去看一下。其中有小伙伴提出，温度检测插件DynamixSystemTemperature不会安装。所以这期教程就教大家安装。为了从零开始演示，我把自己装的插件卸载了。看在我这么敬业的份上还不赶紧点赞收藏搞起来（疯狂暗示）。原[值友96](https://zhiyou.smzdm.com/member/6407050449/)|*赞*70*评论*64*收藏*203[查看详情](https://post.smzdm.com/p/aekeqz2q)

需要先安装插件：Dynamix System Temperature

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d877574d7532.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_20/)

再安装插件：NerdPack

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d878a12f7389.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_21/)

然后跟着教程，设置即可完成，夏天到了，能显示CPU是很需要的！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d879d3909003.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_22/)

15、安装插件：Dynamix System Buttons

APPS里面搜索这个插件，直接安装即可。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

安装了以后，可以实现一个快速重启，关机的按钮：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d87ced5e7815.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_23/)

16、安装插件：Dynamix System Stats

直接在APPS里搜索安装即可：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d87b6cbc6474.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_24/)

这个插件可以显示各种性能参数，帮我们监控UNRAID的占用率：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d87c6d509216.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_25/)

17、设置了UNRAID主题，改成黑色：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

先点击按钮：SETTTINGS——Display settings

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d883d5656719.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_26/)

这里可以设置横幅以及背景色，我觉得黑色就很好看，改成Black：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d880fb423638.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_27/)

18、**这次我重新制作了阵列**：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

一个4T校验盘，一个4T存储盘，两个NVME固态做缓存：

当有2个缓存盘的时候，这2个缓存盘会自动组成raid1，为了数据安全，我觉得很有必要：

（主要是虚拟机下群晖NAS对NVME的直通兼容性不好，所以还是用UNRIAD当存储了！）

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8837d8b4819.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_28/)

19、设置某些文件夹 不使用缓存：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

点击按钮：SHARES，选择文件夹，然后设置：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d883752a3413.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_29/)

20、在有缓存，有万兆网卡的情况下，拷贝文件写入到unRAID上，速度达到了867MB/s，而且很稳：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d88579e25342.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_30/)

21、安装插件：CA Fix Common Problems

这个插件可以检测出，我们的配置有没有问题，APPS里搜索后安装即可。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/39.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d88824452565.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_31/)

安装后，在PLUGING里，可以检查到了一些问题：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/68.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d88c66ee4831.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_32/)

22、安装插件：Dynamix SSD Trim

这个插件是上面检测问题的时候发现的，适合有缓存的用户使用，安装后，可以定期将SSD的资源移动到阵列里：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

直接在APPS里 搜索安装即可：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d88f41ee1861.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_33/)

然后在PLUGINS里面就可以看到这个插件可以设置几点钟把缓存里的文件写入到阵列里，默认是凌晨3点40，一般默认即可：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d88ea3563632.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_34/)

23、也许有人换了硬盘了，想重新弄UNRAID的阵列。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/99.png)

只需要停止阵列，然后点击这个地方，全做all，全部打勾，就可以重置阵列了：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d893753a6337.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_35/)

初始化篇结束，目前就可以开始正式使用unRAID系统了！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/129.png)



## 三、虚拟机篇

我们初始化了UNRAID以后，最先安装虚拟机，等虚拟机稳定后，再安装Docker应用！

虚拟机的文件我已经上传到云盘：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[链接：](https://pan.baidu.com/s/1JnAuUTNkivZmh9hZfijRMg)提取码：rd0p

Win10的镜像在这个链接里面下载，有8个G：

[![img](asset/新建文本文档/5e368cecf2ff74380.jpg_a200.jpg)](https://post.smzdm.com/p/awxq48zg)[**UNRAID教程：1分钟 用自带虚拟机安装 荒野无灯大佬的精简版windows10系统**](https://post.smzdm.com/p/awxq48zg)前言：大家好，俺又来了！昨天，在灯大群里，看到他分享了一个精简版的win10系统，并且支持多个系统：群晖/QNAP/unRAID/ProxmoxPVE可直接导入此vm磁盘镜像win10.qcow2在我下载体验以后，觉得非常方便，于是决定写这篇文章分享给大家。已征得灯大授权：这个win10系统，非常的[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*806*评论*547*收藏*6k[查看详情](https://post.smzdm.com/p/awxq48zg)

将所有虚拟机的文件，先上传到UNRAID的 isos 文件夹下面：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

记得解压 ds3617_6.1的压缩包：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d89179e32250.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_36/)

确保以上步骤都已经搞定！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)



**1、先准备安装爱快软路由：**

爱快软路由我用了PCIE转4网卡，这个网卡很便宜，淘宝就可以购买：

实测可以分开虚拟机直通！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/99.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d88f07484209.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_37/)

2、为了避免网卡直通出现各种问题，我们先参考这个教程，安装一下网卡分离补丁：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

貌似I350T4网卡 不需要安装这个补丁：

[![img](asset/新建文本文档/5e682f3b29b888468.jpg_a200.jpg)](https://post.smzdm.com/p/ax025x24)[**UNRAID下解决华擎 J3455-ITX  IOMMU 分组（4口网卡顺利分开直通 ）**](https://post.smzdm.com/p/ax025x24)小编注：此篇文章来自#原创新人#活动，成功参与活动将获得额外50金币奖励。自己搞定6.8.1开心版后，覆盖unraid群里大神共享的文件，具体为：由蓝冰血魄制作，unRAID•RSG博客群•NAS群江苏-biu协助测试。本module基于unraid6.8.1制作，主要用于解决j3455iommu分[onenote](https://zhiyou.smzdm.com/member/7390105087/)|*赞*29*评论*93*收藏*205[查看详情](https://post.smzdm.com/p/ax025x24)

[补丁的下载链接：](https://pan.baidu.com/s/139QxQ8lE1oUdrD1nwntlaA)提取码：3hme

分离成功后，网卡应该是这样子的：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d89940578988.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_38/)

3、切记，需要设置下面这个：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

settings-- vm Manager -- PCIe ACS override:开启 Downstream

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d89c0f5b6418.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_39/)

屏蔽网卡代码如下：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d898a6e61191.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_40/)

4、然后参考视频教程，安装爱快：









爱快，我直通了两个网口，一个WAN口，一个LAN口。

我爱快设置如下，如果不看上面的教程，很多东西有可能会搞不懂，我这里截图是给大家我的信息，方便大家定位：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/39.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d89982121218.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_41/)

5、网线需要都插上，并且要知道哪个网线是哪个接口。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

这个可以测试出来的，在虚拟机里面拔掉网线，接上网线，对应的连接提示也会亮起：

这样就可以测试出，哪个口对应哪个位置：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d89c3cc68721.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_42/)

6、爱快默认账号密码是：admin、admin

然后绑定wan口，记住哪个网卡是wan口，哪个网卡是lan口。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/39.png)

跟着教程设置好WAN、LAN、HDCP！

然后用爱快拨号，LAN口接交换机，成功上网：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d89ee524138.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_43/)

**7、继续安装openwrt软路由：**

参考视频教程，安装openwrt：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

下面教程大概16分钟左右：



我直通了一个口，我的OPENWRT设置：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8a72b0c1706.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_44/)

8、首次安装openwrt，得先输入输入代码：vi /etc/config/network

修改后台ip地址，完成后记得输入reboot重启：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/39.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8a8efa94094.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_45/)

9、openwrt的默认账号密码是root 和 password：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8a3c5e29158.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_46/)

10、爱快是主路由，openwrt是旁路由，关于旁路由设置我这里必须再提一下：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

爱快需要在DHCP服务端里的 网关设置，填上openwrt 的地址：

DNS 输入114.114.114.114 以及 223.5.5.5

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8a4c4dd4896.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_47/)

11、openwt的设置：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

需要在网关的地方，填写上爱快的地址：

DNS必须和爱快设置的一样：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8aef7317715.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_48/)

12、上面这个页面，往下滑一下，忽略DHCP接口这个打勾，这样就成功开启了旁路模式：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8a81d6d1854.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_49/)

13、到此，软路由就安装成功了：

百度搜索测网速，可以测一下，速度很稳：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8b08f5b8206.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_50/)

14、openwrt里面开启一些神秘插件的功能，就可以顺利进行某视频网站的测速，速度我很满意：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8ae59c150.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_51/)



**15、安装win10 虚拟机**：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

参考教程：

[![img](https://qna.smzdm.com/202002/02/5e368cecf2ff74380.jpg_a200.jpg)](https://post.smzdm.com/p/awxq48zg)[**UNRAID教程：1分钟 用自带虚拟机安装 荒野无灯大佬的精简版windows10系统**](https://post.smzdm.com/p/awxq48zg)前言：大家好，俺又来了！昨天，在灯大群里，看到他分享了一个精简版的win10系统，并且支持多个系统：群晖/QNAP/unRAID/ProxmoxPVE可直接导入此vm磁盘镜像win10.qcow2在我下载体验以后，觉得非常方便，于是决定写这篇文章分享给大家。已征得灯大授权：这个win10系统，非常的[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*806*评论*547*收藏*6k[查看详情](https://post.smzdm.com/p/awxq48zg)

win10 我是把之前 另外一台UNRAID里面的镜像，直接复制到这台里面！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

这台新的UNRAID，安装了WIN10虚拟机以后，发现数据全部都在，软件也都在。

太方便了，win10我就挂了一个百度云，一个相册程序，一个TMM刮削还有一些其它的x86应用。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/43.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8b177c28001.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_52/)

16、我的WIN10虚拟机设置：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/67.png)

和上面教程不同的地方，我这里网卡改成了br0：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8af0a272429.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_53/)

**17、安装黑群晖，并且直通硬盘和网卡：**

参考教程：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![img](asset/新建文本文档/5dfb899e1c6742949.jpg_a200.jpg)](https://post.smzdm.com/p/az50d36r)[**UNRAID教程：3分钟 用unraid自带的虚拟机 安装 黑群晖NAS DSM系统 很强大！**](https://post.smzdm.com/p/az50d36r)前言大家好，俺又来了。相信很多朋友，真的是久等了吧，这已经是我写的第四篇关于UNRAID的文章了。这篇文章内容，是教给大家，如何用UNRAID自带的虚拟机来安装一个群晖NAS系统。（本次文章，非常感谢人生观RSG大佬的热情分享，没有他的帮助，就没法这么容易学习到安装群晖的方法，谢谢大佬，大家可以在百[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*290*评论*295*收藏*807[查看详情](https://post.smzdm.com/p/az50d36r)

直通教程参考：

[![img](asset/新建文本文档/5e49766d326bb3394.jpg_a200.jpg)](https://post.smzdm.com/p/awx0nglg)[**家用媒体服务器NAS 使用UNRAID系统的正确的玩法！直通网卡、直通硬盘、挂载群晖虚拟机文件！**](https://post.smzdm.com/p/awx0nglg)前言大家好，俺又来了！本文内容有点长，而且十分干货，建议先收藏，再观看！最近一直忙着出威联通相关的折腾文章，忽略了unraid系统了。其实很早就开始构思这篇文章了，因为unraid的玩家越来越多了！！越早告诉大家这种方式，就能让更多人避免进入到一种坑里，到时候转换数据又很麻烦！本文目录：关于UNRA[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*421*评论*447*收藏*2k[查看详情](https://post.smzdm.com/p/awx0nglg)

直通硬盘需要添加下面代码，后面的数字是硬盘名字：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/99.png)

> /dev/disk/by-id/ata-WDC_WD100EZAZ-11TDBA0_JEJSMRTN

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8b7094f5800.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_54/)

18、网口直接直通打勾后，无需修改教程配置里的 E1000设置，直接可以通过群晖助手找到IP地址：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8bb2898199.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_55/)

后续可以把虚拟的网口信息给删掉：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

编辑虚拟机的地方，右上角的按钮点一下，就可以看到这些代码，找到 interface等字样，删掉这些：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed14ca03ff08363.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_56/)

19、我的虚拟机 黑群晖设置：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8ba0ac09727.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_57/)

20、群晖成功安装，正在校验磁盘：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8b842551013.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_58/)

21、校验完成后，测试了下速度，直通了硬盘和网口后，跑满千兆：112MB/s

传输非常非常的稳：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8bda42e3572.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_59/)

22、群晖安装的应用如下：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

主要跑相册，Docker用来pt下载，记事本也比较好用，然后Cloud Sync可以将相册备份到云端：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8bb6eff2688.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_60/)

23、群晖在手机安装了相册APP自动上传照片以后，会进行略缩图转码，这个时候的虚拟群晖占用达到了99%，不过由于只给了3个核心给群晖，所以整体不会影响到UNRAID的软路由等设备：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8c14f365532.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_61/)

24、群晖的相册是真好用，由于这台设备是24小时开机，所以我会用来自动备份手机照片。

搭配Cloud Sync 可以把照片再备份到云端。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

后续等有空了，再手动备份到威联通TVS-951N里面，这样我的照片就几乎是保存了3个地方，非常安全！而且我还有一个移动硬盘，用来冷备，更加安全！

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8c628af6380.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_62/)

25、特别提醒，群晖的相册正确的玩法，推荐先备份照片到Photo文件夹，再用moment打开共享相册进行人脸识别，场景识别，这样会方便后续整理照片。

之前我也写过文章：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![img](asset/新建文本文档/5e4aa0b6ad50d3278.jpg_a200.jpg)](https://post.smzdm.com/p/av7zn43p)[**群晖NAS相册的正确玩法！使用Moments的朋友必看！强烈推荐搭配DS Photo使用！**](https://post.smzdm.com/p/av7zn43p)前言大家好，俺又来了！现在很多朋友玩unraid系统，而大多数朋友，最希望能有一个非常好用的相册管理软件！比如群晖的Moments，不知道Moments的朋友可以看看下面本站@bill_bill的文章：我上篇用虚拟机直通的方式，在unraid下使用群晖NAS系统，这样就可以完美的使用群晖和相册软件了[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*447*评论*545*收藏*4k[查看详情](https://post.smzdm.com/p/av7zn43p)

26、UNRAID下的虚拟机群晖用户，还可以安装一个这个工具，来自@[usee00123](https://zhiyou.smzdm.com/member/9934421314/)教程：

[![img](asset/新建文本文档/5ec638232dd2b1695.jpg_a200.jpg)](https://post.smzdm.com/p/ax02zpq4)[**我的NAS方案最终篇——从抛弃蜗牛开始**](https://post.smzdm.com/p/ax02zpq4)小编注：此篇文章来自#原创新人#活动，成功参与活动将获得额外50金币奖励。前言：19年春节，跟着蜗牛矿难的脚步入手了人生第一个nas设备，蜗牛C单（J1900+4G+16G），安装了黑群和各种套件，尝试了docker和虚拟机应用，作为服务器稳定使用了几个月。蜗牛便宜好用但缺点也明显，机箱风扇和电源风[usee00123](https://zhiyou.smzdm.com/member/9934421314/)|*赞*144*评论*119*收藏*794[查看详情](https://post.smzdm.com/p/ax02zpq4)

安装了以后，直接在UNRAID里关机，会自动关闭群晖里的系统，非常的方便：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8c69a927392.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_63/)

27、到此，四个虚拟机就都安装好了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/39.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8c3ae88281.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_64/)

28、网口一览：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8c5f8c56297.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_65/)



## 四、测试环节

1、功耗测试：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

（此功耗测试时，还没插PCIE网卡，而且是低负载下测试，仅供参考）

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8ca5d794155.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_66/)

2、unRAID的特色是 硬盘支持休眠，而且可以单盘休眠，在休眠后，功耗还会降低：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8cc004d8303.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_67/)

3、休眠后功耗只有25w：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8ce4ceb4316.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_68/)

4、休眠的设置地方：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8d1ade84350.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_69/)



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8d246877266.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_70/)

5、硬盘温度也不错：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

机械硬盘30°、NVME温度在40°左右、处理器39°

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8d476787317.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_71/)



## 五、安装Docker应用

1、首先安装Emby、Jellyfin、Plex，这些媒体服务器：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

参考教程：

[![img](https://qna.smzdm.com/201912/19/5dfa5cf35a4285097.jpg_a200.jpg)](https://post.smzdm.com/p/a25gpmpn)[**UNRAID教程：3分钟 用安装Jellyfin 开启硬件加速转码 解码4K 打造最强家庭影院**](https://post.smzdm.com/p/a25gpmpn)前言大家好，俺又来了！这是我写的关于UNRAID第三篇文章了，依旧是保姆级文章，搭配前面两篇食用，效果会更佳。最近站里分享UNRAID的文章越来越多，这是一件非常好的事情。玩的人越多，就越多精彩的内容，已经看到不少干货文章出现了！今天教给大家如何用UNRAID开启集成显卡的硬件加速功能，让Jelly[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*486*评论*364*收藏*3k[查看详情](https://post.smzdm.com/p/a25gpmpn)

unRaid的特点就是硬件转码调取很方便，之前初始化的地方，也已经加入了集成显卡驱动。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

可以确定一下，是不是新建了一个Movie文件夹，以后用这个文件夹来存放影片。

或者到时候挂载虚拟群晖下面的文件夹：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/43.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8d3a5c09060.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_72/)

2、因为我买了emby，先安装emby：

Emby的镜像有很多，选择 linuxserver的这个镜像：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/34.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8d5ce267920.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_73/)

3、弹出这个提示，点 latest 最新版：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8d8e3964392.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_74/)

4、点击后，只需要设置一个Movie的路径，设置我们新建的movie路径，或者改成挂载的群晖路径：

然后我们还需要添加一个集成显卡驱动，这样才方便硬件转码：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/43.png)

点击最下面的 Add XXXX

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8db536d7061.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_75/)

5、这样设置后，就可以APPLY 应用这个Docker了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8dc924f1544.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_76/)

6、以上我的所有设置，都可以参考刚刚分享的教程。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

注意，Emby是需要收费买会员才能开启硬件转码的，免费用户还是推荐用Jellyfin！

安装成功：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8de49865795.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_77/)

7、开启硬件转码：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8de84db3909.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_78/)

8、测试一下，硬件转码毫无压力，很给力！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8de8313599.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_79/)

9、挂载群晖的文件夹：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

接下来，再来演示下，安装Jellyfin，这个Jellyfin，我打算用来挂载虚拟机下的黑群晖的路径！

在MAIN里面，先挂载一下黑群：

需要安装插件：Unassigned Devices

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8e4b6d95083.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_80/)

10、如果弹出这个提示，点这个右侧的这个SMB连接：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8e61b705029.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_81/)

11、然后点击扫描的地址后，选择虚拟群晖地址，点下一步：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/67.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8e665144140.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_82/)

12、然后需要设置账号密码，输入一下，到这个页面，直接点击下一步：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8e9033c6596.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_83/)

13、然后读取可以挂载的文件夹，选择我们在群晖里面建立的文件夹后，点击完成：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8eaf1f6785.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_84/)

14、完成以后，还要点击这个按钮进行挂载：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8e9e6c81617.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_85/)

15、挂载成功，就可以看到剩余的空间了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8eece676803.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_86/)

16、点击中间的这个DSM8400_Movie按钮，就可以跳出到这个路径：

记住这个路径：/mnt/disks/DSM8400_Movie

后续就是在这个路径来进行映射！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8f0e2518949.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_87/)

17、安装Jellyfin，APPS里面搜索Jellyfin，然后还是要选择linuxserver这个作者的镜像：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8ed81bd9358.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_88/)

18、和Emby一样，需要设置一个路径，路径设置我们刚刚挂载的，然后这个端口我们需要改一下，不然会和Emby冲突：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

然后一样要添加一个显卡驱动：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8f2eea69537.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_89/)

19、显卡驱动和Emby的添加一毛一样：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8f23dcc2741.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_90/)

20、这样，两个就不冲突了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8f4da617455.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_91/)

21、在Jellyfin后台，开启硬件转码：



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8f6a70a7799.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_92/)

22、将影片上传到虚拟黑群晖的文件夹内：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8f966334761.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_93/)

23、创建好，可以添加不同路径的媒体夹：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8fac38f3073.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_94/)

24、测试播放，转码毫无压力：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8f919eb2575.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_95/)

25、接下来安装PLEX，这个我买了，当然是要用起来了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

PLEX教程参考这篇文章：

[![img](asset/新建文本文档/5e3fb82f3eedc8878.jpg_a200.jpg)](https://post.smzdm.com/p/amm5rmrp)[**界面漂亮，全平台通用：300元购买 PLEX Pass会员！打造私人 家庭影院 媒体服务器！**](https://post.smzdm.com/p/amm5rmrp)大家好，俺又来了！今天给大家介绍一款家用的影音媒体管理器，PLEX！PLEX和Jellyfin很像，都是将NAS或者电脑里面下载的影片进行封面处理，然后进行管理。下载的影片一般是下图这样的，当用了PLEX后，就变成封面那样子了：PLEX的教程，站内有很多，而且全平台支持，不管您是群晖、还是威联通、还[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*463*评论*540*收藏*3k[查看详情](https://post.smzdm.com/p/amm5rmrp)

依旧选择linuxserver大佬的镜像：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8fdeeb66061.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_96/)

26、和上面的安装方式一样：

挂载了黑群NAS的路径，然后添加集成显卡驱动，和上面一模一样：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/34.png)

只需要设置这几个地方，其它的别动：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d8fd31467576.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_97/)

27、安装PLEX后，设置开启硬件转码功能：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d90019ed4154.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_98/)

28、转码测试也是一切正常，在PLEX后台，看到有 小括号(hw)的，就代表正在硬件转码：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d900adae8698.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_99/)

29、到此，三大媒体软件就安装完成了！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

这里点评一下，Plex没有Emby和Jellyfin共享影片方便，属于我自己独享的媒体平台！

不过还是有很多朋友比较喜欢PLEX，这个没有办法！我更推荐新手选择购买Emby或者直接用免费的Jellyfin！

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d901ead42376.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_100/)

30、接下来安装QB下载工具，参考教程：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![img](asset/新建文本文档/5eafde100f7737092.jpg_a200.jpg)](https://post.smzdm.com/p/av7m3nw4)[**威联通Docker教程 篇八：三分钟安装QB离线下载工具，下电影，PT，挂种利器！Qbittorrent NAS专用下载工具！**](https://post.smzdm.com/p/av7m3nw4)想攒一台电竞主机、家用主机、酷炫主机无从下手？想省钱又怕性能不达标？值得买帮你打造定制化DIY装机工具，自助全网比价装机，提供最适合的搭配方案，解决各种攒机场景下难题。>快快使用戳这里点击这里查看详情[阿文菌](https://zhiyou.smzdm.com/member/6902738986/)|*赞*709*评论*165*收藏*4k[查看详情](https://post.smzdm.com/p/av7m3nw4)

QB我用的是荒野无灯大佬的，这里我选择参考的是威联通下搭建的方法：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/43.png)

我会在虚拟群晖里面的Docker安装QB，这样因为直通了硬盘和网卡，就不用再占用UNRAID的资源：

在群晖里，安装了Docker后，搜索：qbittorrent

找到 80x86 荒野无灯大佬的 qb镜像：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d90424fb941.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_101/)

31、选择一个版本号，要用amd结尾的才可以，不要用latest：

我用的个人觉得很稳定的4.2.1版本![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d907db605296.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_102/)

32、在下载镜像的时候，我们最好设置一下群晖的文件夹权限，把Docker 和 Movie都添加Everyone全部可读写权限：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/22.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d907bf79942.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_103/)

33、等待镜像下载完成，我们启动镜像，点击高级设置，先设置路径：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d90b9dc71757.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_104/)

34、然后设置端口：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/22.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d90de94e9426.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_105/)

35、再设置环境变量，群晖下只修改一个8089 的端口就可以了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d90ae0601591.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_106/)

36、设置完成后，启动了容器后，在浏览器输入 黑群晖NAS的IP地址+端口8089就可以访问QB了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/38.png)

QB默认账号密码是 admin adminadmin

其它设置参考之前的文章即可，成功下载，速度很快，可以跑满家里的带宽。

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d90d57af665.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_107/)

37、然后给UNRAID安装文件管理器：

首先安装FB文件管理器，使用的也是荒野无灯大佬的：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![img](asset/新建文本文档/default.png)](https://hub.docker.com/r/80x86/filebrowser)[**DockerHub**](https://hub.docker.com/r/80x86/filebrowser)[hub.docker.com](https://hub.docker.com/r/80x86/filebrowser)[去看看](https://hub.docker.com/r/80x86/filebrowser)



荒野无灯大佬最新更新了2.9.4版本，这个版本，略缩图生产速度提高了2-4倍，CPU占用也减少了2倍以上。我直接在unRAID的Docker里面安装：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

设置如下，添加了名字以后，需要添加端口和一些路径设置，我下面都有留中文：

相关参数：

> filebrowser
>
> 80x86/filebrowser:2.9.4-amd64
>
> 8082
>
> /config
>
> /mnt/user/appdata/fb
>
> /myfiles
>
> /mnt/user/



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed14f5911bc33736.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_108/)



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed14f375d7526374.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_109/)



38、如果还看不懂，看一看这个图：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

主要设置的是Container Path 和 Host Path 其它可以随意填写：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d91039fd1012.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_110/)

39、很简单就可以安装好FB，输入UNRAID的IP地址+端口8082 即可访问：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

默认账号密码 admin admin

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d91597381577.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_111/)

40、如果有集成显卡驱动的，可以添加显卡代码，这样用FB播放视频，就可以启用显卡加速：

如果FB只是用来文件存储，那么下面这个可加可不加。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d913be564449.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_112/)



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d9172bea2823.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_113/)

41、然后heimdall导航页，参考教程：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![img](asset/新建文本文档/5e33c1994de463421.jpg_a200.jpg)](https://post.smzdm.com/p/andgr3wv)[**unraid 篇二：来教大家在unraid上安装自己的私人导航网站**](https://post.smzdm.com/p/andgr3wv)小编注：此篇文章来自#原创新人#活动，成功参与活动将获得额外50金币奖励。Unraiddocker第二期昨天出去放风了，今天闲着无事，更新一期文章吧，来教大家在unraid上安装自己的私人导航网站，好了，教程开始：用unraid大家想必都会安装很多的docker软件，但是安装多了，访问起来你要记住很[丢丢的生活记录](https://zhiyou.smzdm.com/member/3546185883/)|*赞*24*评论*37*收藏*160[查看详情](https://post.smzdm.com/p/andgr3wv)

直接在Apps 搜索：heimdall

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d9180d296924.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_114/)

42、设置两个端口，分别是1900 和 1901：

以及添加一个配置路径：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d9191d3c2834.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_115/)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed14fbb8aa0c7516.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_116/)

43、配置的路径这样设置：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d91ad9994371.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_117/)

44、安装完成后，在浏览器输入UNRAID的IP+端口1900 成功进入导航页：

其它跟着教程设置，即可制作好导航页，这个导航页很方便：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d91e507c530.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_118/)

45、有个技巧，添加一个账户，把这2个打勾，这样就可以更方便的使用这个导航页：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d91d3a863399.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_119/)

46、安装荒野无灯大佬的博客，用来当主导航页：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

为什么要这么做，主要是UNRAID下，我不想安装宝塔等工具来做网页，还是直接图方便弄个博客，搭配导航页主题比较实在：

typecho 还是很不错的，安装也超级简单，Docker设置如下：

> 80x86/typecho
>
> /data
>
> /mnt/user/appdata/typechodhy



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d92494b63004.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_120/)



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed150a72b96a7446.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_121/)

47、对了，用IP+端口访问typecho的时候，只需要设置密码，其它请保持默认！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d9259a549699.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_122/)

48、百度搜索 typecho 导航[主题](https://www.seogo.me/muban/webstack.html#comment-8961)，找到对应的主题：

我已经下载好了，[链接：](https://pan.baidu.com/s/1BcjCTOgdYMQncESbsB512g)提取码：f6pn

将下载的主题，用FB文件管理器，放到 appdata--typechodhy--themes里面，并且记得解压：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d92566d89648.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_123/)

49、进入到typecho后台，在外观设置里面，设置主题：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d927591e4685.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_124/)

50、新建一篇文章，标题写上对应的名字，上传一个LOGO图片，LOGO地址填写刚刚上传的路径即可，然后填上跳转的链接和描述，发布文章：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/60.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d928efea8192.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_125/)

51、完成后的效果展示，欢迎访问[我的导航页](http://www.junwen.bid:1800/)！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d929406e5035.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_126/)

52、要返回到管理台页，在链接后面添加 /admin/index.php 后缀即可，如下，就可以进入后台了：

> 10.10.10.251:1800/admin/index.php

其它LOGO修改，类型修改，都要看自己优化了，我个人觉得这个导航页虽然不够美观，但是完全够用。

最主要是方便了很多，总比搭建宝塔，再放网页要轻松很多：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d92caf602690.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_127/)

53、安装个人博客、第二博客，这个安装方式同上，只是路径不一样，端口不一样：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d930f5f89839.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_128/)

54、我之前有备份过博客，只需要在appdata里面，把这些文件夹打包拷贝走就可以了，现在将之前的博客还原，数据就能全部回来：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/68.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d93074177380.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_129/)

55、还原数据之前，记得先停止unraid里面的docker：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

数据全部回来了！O Yes，这也是我为什么坚持用Docker的原因，我的博客数据伴随着我从群晖到威联通、再到unRAID，迁移真的太方便了：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d932510c8461.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_130/)

54、安装在线音乐播放器，我的设置如下：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

> oldiy/music-player-docker
>
> /var/www/html/cache
>
> /mnt/user/appdata/music/



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d9320c3b7738.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_131/)



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed150ca4ae9a7122.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_132/)

55、音乐播放器安装完成：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d933cf249205.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_133/)

56、安装Zdir个人网盘，我的设置如下：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

> baiyuetribe/zdir
>
> /var/www/html/var
>
> /mnt/user/appdata/zdir/zdir



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d93690569494.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_134/)

57、Zdir安装完成：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/60.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d939afe91255.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_135/)

58、安装可道云文件管理器，我的设置如下：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

> baiyuetribe/kodexplorer
>
> /var/www/html
>
> /mnt/user/appdata/zdir
>
> /var/www/html/zdir
>
> /mnt/user/appdata/zdir/zdir



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d93a12939989.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_136/)

59、可道云安装完成：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d93bf0167959.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_137/)

60、安装荔枝相册，我的设置如下：

> 80x86/lychee
>
> /conf
>
> /mnt/user/appdata/lychee/conf
>
> /uploads
>
> /mnt/user/appdata/lychee/uploads



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d941c6ca2208.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_138/)

61、荔枝相册安装完成：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d9421de82320.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_139/)

62、美化一下Docker的图标，参考教程：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![img](asset/新建文本文档/5eba5c7f65fad7862.jpg_a200.jpg)](https://post.smzdm.com/p/a6lrdeen)[**unraid折腾笔记 篇十一：自定义unRAID虚拟机&docker图标icon，强迫症患者表示终于舒服了**](https://post.smzdm.com/p/a6lrdeen)4月27日-6月21日，参与#种草大会#征稿活动，帮助值友种草好物，每周文章热度TOP5奖励300元购物卡，成为种草王最高可得3000元购物卡，点击查看活动详情。写在前面使用unraid的同学都知道，由于服务器在国外，我们经常打不开app市场。因此我们自己安装的docker默认都是没有icon图标的[值友96](https://zhiyou.smzdm.com/member/6407050449/)|*赞*39*评论*24*收藏*243[查看详情](https://post.smzdm.com/p/a6lrdeen)

也许有人不知道这些图标地址在哪里找，给大家一个提示，可以选择自己的导航页的图标地址。

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d945c4f59767.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_140/)

63、到此，我们的Docker，虚拟机，就全部安装完毕了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d946be295985.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_141/)



## 六、外网访问和Aliddns

1、很多人问我，怎么外网访问NAS：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

我是电信用户，直接打电信10000号码，说家里安装了服务器，需要公网ip。

然后申请到了公网IP，光猫改了桥接模式以后，给软路由的爱快来进行拨号：

如果有了公网ip，在百度搜索本机IP，记住这个IP地址：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/60.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d945e17d403.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_142/)

2、在爱快里面设置一下端口转发，切记无法转发外网80或者443端口，请转成别的端口：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/99.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d94d10185013.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_143/)

3、端口转发弄好后，输入公网IP+端口，就可以在外网访问家里的设备了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/109.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d94ed68b3599.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_144/)

4、然后我们申请一个阿里云的域名，添加A记录，将公网IP的地址填上：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d95010b06767.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_145/)

5、然后等了10分钟左右，域名解析了好了，就可以通过域名+端口的形式访问家里的设备了：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/39.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d94e5452517.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_146/)

6、由于家用的公网IP，一般不是固定的IP，当爱快重新拨号后，这个IP地址就会变化！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/36.png)

所以需要搭配一个能自动修改阿里云域名 A记录解析的插件。

登录阿里云后，点击自己的头像，找到这个AccessKey管理选项：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d950f16b7217.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_147/)

7、创建一个密钥：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/67.png)

记住这个密钥的ID 和 Key值：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d957b6673654.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_148/)

8、一般路由器有aliddns的插件，可以直接用上面这组密钥来设置DDNS插件。

现在我都是用的Docker来实现这个功能。

在Docker里面添加下面这个镜像：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/99.png)

> chenhw2/aliyun-ddns-cli
>
> AKID
>
> AKSCT
>
> DOMAIN
>
> REDO



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed209622430d7077.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_149/)

9、添加方式如下，我都做了很详细的描述：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/109.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d9590bf74343.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_150/)



[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d956c5d28555.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_151/)

10、按照上面设置以后，就算以后公网IP发生了变化，这个Docker也会帮我们自动修改阿里云的解析IP地址，这样就可以长久使用域名+端口的方式访问家里的设备了！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d959da6d5769.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_152/)



## 七、UNRAID的备份和还原

1、全部设置完成后，我们要备份一下U盘信息，这样就算U盘损坏，我们的UNRAID也没有问题的：

点击MAIN找到Flash U盘：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/35.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d95ce6ae8071.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_153/)

2、然后点击备份按钮：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/54.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d95e54317585.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_154/)

3、备份大概要几分钟，备份文件是用ZIP格式保存的，直接下载到本地电脑里。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

其实就是将U盘里的数据打包了，以后保存好这个备份文件，当U盘出现故障的时候，换一个U盘，就不影响之前的设置了：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d960eb6f2626.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_155/)

4、如何还原U盘备份的文件呢？![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/104.png)

我们用unRAID的安装工具，选择到 Local Zip，然后选择之前备份的ZIP文件，即可还原：

unRAID的U盘里，只保存了一些设置，至于Docker镜像和虚拟机文件，都是保存在硬盘里的：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d96111d823.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_156/)



## 八、遇到的坑

如果没有直通openwrt的用户，在内网下，有可能导致应用网络不通，需要下面这样设置一下：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/99.png)

将网关改成主路由的IP地址即可：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d963ecc53042.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_157/)

如果UNRAID遭遇断电，只要是非自然关机，校验盘就需要重新校验一次，一次得很久很久：![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/78.png)

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed13d966bad81558.png_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_158/)

如果APPS老是进不去，有可能需要换网关，或者换Host，或者无线刷新网页，直到能进APPS！

APPS里面的东西一旦设置好了， 后续就很少会用到APPS的功能了！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/99.png)

关于爱快的端口映射，爱快自身如果要映射出去，记得在 系统设置——远程访问里面 把WEB访问控制打开：

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed1fd37d70322094.jpg_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_159/)



## 总结

到此，UNRAID的折腾，就全部结束了！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png) 是不是巨复杂？

其实是我安装的东西比较多，大家如果安装东西较少的话，就很容易使用了！

后续再安装什么功能，就是大家自己的想法了！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/40.png) 写上万个字了，真心不容易！

[![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/5ed3d143b32a59769.jpg_e680.jpg)](https://post.smzdm.com/p/ag87w953/pic_160/)

文章中有很多教程的跳转链接，如果您想好好的学习UNRAID系统，那么多看，多尝试再提问！

也欢迎解答专家，到这篇文章的评论区帮忙回答问题！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/55.png) 真的感激不尽！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/128.png)

最后，感谢大家的观看，unRAID确实是一款非常优秀的NAS系统！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/33.png)

希望大家找到优秀的Docker应用，好用的虚拟机，unRAID的技巧，踊跃在什么值得买平台发文章，相信你的善意的动作，可以帮助到大多数朋友！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/64.png)

为了写好这篇文章，我将站内所有关于unRAID的文章全部看了。。。。。![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](https://res.smzdm.com/images/emotions/56.png)

如果您遇到问题，无法解决，也记得善用搜索！

**最后的最后，希望大家为我的文章，点赞、收藏、打赏一下下，感谢大家的支持！**

后续我还会分享更多有意思的文章，希望没关注的朋友，也点下关注，我们下次再见！![NAS最强攻略：使用UNRAID系统，搭建ALL IN ONE全过程！超万字教程，绝对干货！](asset/新建文本文档/35.png)