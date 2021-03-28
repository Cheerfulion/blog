---
title: 我的hexo博客搭建记录
description: 暂无描述！
tags:
  - hexo
abbrlink: fb846fae
date: 2021-03-28 23:44:27
---



搭建网上的教程很多，我这里举例一个：

[超详细Hexo+Github Page搭建技术博客教程【持续更新】](https://segmentfault.com/a/1190000017986794)

我这里使用的是hexo-yilia主题，只写下了搭建好hexo博客和yilia主题后的进阶操作。



## 头像和网站logo设置

**1、存放位置**
头像、图标图片的存放位置是`/themes/yilia/source/`下任意位置，可以自己新建一个文件夹存放，我存放在`assets/img`文件夹下。

**2、配置设置**
配置文件为`/themes/yilia/_config.yml`。

设置头像为配置文件中`avatar`一项

设置网站logo为配置文件中`favicon`一项

由于项目中`_config.yml`路径的根目录为`/blog/`（`root: /blog/`），因此，修改配置为：

```yml
# /themes/yilia/source/assets/img/favicon.jpg
favicon: /blog/assets/img/favicon.png

# /themes/yilia/source/assets/img/avatar.jpg
avatar: '/blog/assets/img/avatar.jpg'
```



## 设置标题等站点信息

`config.yml`

```yml
# 站点信息设置
title: ionluo的个人博客站点
subtitle: '记录生活和技术的点点滴滴'
description: '每一滴积累，都是日后的财宝！one piece 乾杯! '
keywords: '个人博客 ionluo 前端 后端 全栈开发 虚拟现实 游戏开发 数学建模 数据分析'
author: ionluo
language: en
timezone: ''

# 通常我们的github page不仅仅只有一个，所以最好root不要设置成 /
url: https://cheerfulion.github.io/blog/
root: /blog/
```



## 首页列表展示部分内容

这里我参考了：https://blog.csdn.net/itguangzhi/article/details/79510044

https://blog.csdn.net/weixin_30399797/article/details/99023570

但是第二个好像不行，对第一种的方法做了点调整。



编辑`themes\yilia\layout\_partial\article.ejs`文件，

```ejs
	  <% if (post.excerpt && index){ %>
        <%- post.excerpt %>
        <% if (theme.excerpt_link) { %>
          <a class="article-more-a" href="<%- url_for(post.path) %>#more"><%= theme.excerpt_link %> >></a>
        <% } %>
      <% } else { %>
        <!-- 首页不展示文章所有内容开始 -->
        <% if (theme.auto_excerpt && theme.auto_excerpt.enable && index) { %>
          <%- post.content.substring(0, theme.auto_excerpt.length) %>...
          <% if (theme.excerpt_link) { %>
            <a class="article-more-a" href="<%- url_for(post.path) %>#more"><%= theme.excerpt_link %> >></a>
          <% } %>
        <% } else { %>
          <%- post.content %>
        <% } %>
        <!-- 首页不展示文章所有内容结束 -->
      <% } %>
```



理论上来说更推荐增加`<!-- more -->`的形式，或者文章头部指定`description`. 因为这里直接截取有可能带上markdown的格式而不仅仅是纯文字，会导致页面混乱。





## 置顶文章

1. **安装插件**

   ```bash
   npm uninstall hexo-generator-index --save
   npm install hexo-generator-index-pin-top --save
   ```

2. **配置文章**

   然后在需要置顶的文章的Front-matter中加上top选项即可。top后面的数字越大，优先级越高。

   ```markdown
   ---
   title: xxxx
   date: 2021-03-27 16:10:03
   top: 1
   ---
   ```

3. **配置优先级**

   修改根目录配置文件`/_config.yml`中的`index_generator`。 top值-1标示根据top值倒序（正序设置为1即可），同样date也是根据创建日期倒序。

   ```yml
   index_generator:
     path: ''
     per_page: 10
     # order_by: -date
     order_by:
       top: -1
       date: -1
   ```



## 增加不蒜子统计

利用这个统计，可以知道你博客的访问量

1. **在 `themes\yilia\layout\ _partial\after-footer.ejs`最后添加：**

   ```ejs
   <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
   ```

2. **在`themes/yilia/layout/_partial/article.ejs`中增加代码**

   ```ejs
   <%- partial('post/title', {class_name: 'article-title'}) %>
   <!-- START 显示阅读次数-->
   <% if (!index && post.comments){ %>
   <br/>
   <a class="cloud-tie-join-count" href="javascript:void(0);" style="color:gray;font-size:14px;">
       <!-- <span class="icon-sort"></span> -->
       <span id="busuanzi_container_page_pv">
           阅读数: <span id="busuanzi_value_page_pv"></span> &nbsp;&nbsp;
       </span>
   </a>
   <% } %>
   <!-- END 显示阅读次数-->
   ```



## 增加版权声明

1. **在`themes/yilia/layout/_partial/article.ejs`添加版权声明代码**

   ```ejs
   <!-- 增加版权声明 -->
   <%
   var sUrl = url.replace(/index\.html$/, '');
   sUrl = /^(http:|https:)\/\//.test(sUrl) ? sUrl : 'https:' + sUrl;
   %>
   <% if ((theme.declare_type === 2 || (theme.declare_type === 1 && post.declare)) && !index){ %>
   <div class="declare">
       <strong>本文作者：</strong>
       <% if(config.author != undefined){ %>
       <%= config.author%>
       <% }else{%>
       <font color="red">请在博客根目录“_config.yml”中填入正确的“author”</font>
       <%}%>
       <br>
       <strong>本文链接：</strong>
       <a rel="license" href="<%=sUrl%>"><%=sUrl%></a>
       <br>
       <strong>版权声明：</strong>
       本作品采用
       <a rel="license" href="<%= theme.licensee_url%>"><%= theme.licensee_name%></a>
       进行许可。转载请注明出处！
       <% if(theme.licensee_img != undefined){ %>
       <br>
       <a rel="license" href="<%= theme.licensee_url%>"><img alt="知识共享许可协议" style="border-width:0" src="<%= theme.licensee_img%>"/></a>
       <% } %>
   </div>
   <% } else {%>
   <div class="declare" hidden="hidden"></div>
   <% } %>
   <!-- 增加版权声明结束 -->
   
   <% if ((theme.reward_type === 2 || (theme.reward_type === 1 && post.reward)) && !index){ %>
   ```

2. **增加相关css代码**

   在`theme/yilia/source/main.0cf68a.css`文件下面增加如下css代码即可：

   ```css
   .declare {
       background-color: #eaeaea;
       margin-top: 2em;
       border-left: 3px solid #ff1700;
       padding: 1.5em;
   }
   ```

   从该文件可以看出是一个打包后的压缩css文件，那么源文件在哪里呢？下面就说明下源文件的设置编译方法。

   > 我这里安装sass依赖说不支持我的window10 64bit 的系统，估计是node-sass现在已经不推荐的原因之一。**不打算深入了解主题源码修改的可以不用看下面了。**

   增加文件`themes/yilia/source-src/css/declare.scss`

   ```scss
   .declare {
       background-color: #eaeaea;
       margin-top: 2em;
       border-left: 3px solid #ff1700;
       padding: 1.5em;
   }
   ```

   在`themes/yilia/source-src/css/main.scss`尾部增加下面代码

   ```scss
   @import "./declare";
   ```

   接下来，npm install 安装依赖， npm run dist打包即可。

   > 这里说下打包失败怎么还原，其实就是依赖ejs模板引擎，去对照源码，修改layout下的一些地方即可。主要注意全局的`_partial`文件夹下的模板。

3. **配置主题`themes/yilia/_config.yml`**

   ```yml
   #版权基础设定：0-关闭声明； 1-文章对应的md文件里有declare: true属性，才有版权声明； 2-所有文章均有版权声明
   #当前应用的版权协议地址。
   #版权协议的名称
   #版权协议的Logo
   
   declare_type: 2
   licensee_url: https://creativecommons.org/licenses/by-nc-sa/4.0/
   licensee_name: '知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议'
   licensee_img: https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png
   ```

   如果`declare_type`为1，则在需要添加声明的文章头部增加属性`declare:true`即可。

   > 这里我licensee_url和licensee_img都打不开，算了，用来撑撑场子用而已。

## 私密文章加密

1. **安装插件**

   ```bash
   npm install hexo-blog-encrypt
   ```

2. 配置项目`_config.yml`，末尾添加

   ```yml
   ## 文章加密 hexo-blog-encrypt
   encrypt:
       enable: true
   ```

3. 然后在想加密的文章头部添加上对应字段，如:

   ```markdown
   ---
   title: hello world
   date: 2016-03-30 21:18:02   
   tags:       
   password: 12345
   abstract: Welcome to my blog, enter password to read. 
   message: Welcome to my blog, enter password to read.     
   ---
   
   # password: 是该博客加密使用的密码
   # abstract: 是该博客的摘要，会显示在博客的列表页
   # message: 这个是博客查看时，密码输入框上面的描述性文字
   ```




## 404 页面

这里配置的是腾讯公益404页面

在博客根目录 /source 文件夹下创建404.html

```html
---
layout: false
title: "404"
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 这里需要根据自己的博客自行设置 -->
    <link rel="icon" href="/blog/assets/img/favicon.png">
    <title>IonLuo Blog</title>
</head>
<body>
    <!-- 这里需要根据自己的博客自行设置 -->
    <script type="text/plain" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" charset="utf-8" homePageUrl="/blog/" homePageName="回到我的博客主页"></script>
    <script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
    <script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
</body>
</html>
```

> 重点：
>
> 1. 在本地无法打开的页面链接不显示404页面，但是部署到github上是可以的。本地需要预览可以打开`config.url` + 404.html查看。
> 2. 有些地方这里是用的`http://www.qq.com/404/search_children.js`文件，但是这个不支持https。



## 增加阅读更多效果

Hexo 整合 OpenWrite 平台的 readmore 插件,实现博客的每一篇文章自动增加阅读更多效果,关注公众号后方可解锁全站文章,从而实现博客流量导流到微信公众号粉丝目的.

这里我没有公众号，就不配置了。详细请看插件介绍：[hexo-plugin-readmore](https://github.com/snowdreams1006/hexo-plugin-readmore)



## 添加看板娘

1. **安装插件**

   ```bash
   npm install hexo-helper-live2d --save
   ```

2. **安装模型包**

   作者提供了三个下载模型的办法，我选择操作比较简单的一种
   `npm 模块名` 的方法

   作者提供以下模型的模型包，模型包预览地址见下面的链接，选择你想用的模型，记住名字，选择对应的后缀模型包

   [作者各种模型包展示](https://huaji8.top/post/live2d-plugin-2.0/)

   ```
   live2d-widget-model-chitose
   live2d-widget-model-epsilon2_1
   live2d-widget-model-gf
   live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
   live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
   live2d-widget-model-haruto
   live2d-widget-model-hibiki
   live2d-widget-model-hijiki
   live2d-widget-model-izumi
   live2d-widget-model-koharu
   live2d-widget-model-miku
   live2d-widget-model-ni-j
   live2d-widget-model-nico
   live2d-widget-model-nietzsche
   live2d-widget-model-nipsilon
   live2d-widget-model-nito
   live2d-widget-model-shizuku
   live2d-widget-model-tororo
   live2d-widget-model-tsumiki
   live2d-widget-model-unitychan
   live2d-widget-model-wanko
   live2d-widget-model-z16
   ```


   选择好对应的模型，使用 `npm install 模型的包名`来安装，比如我选择的的是`live2d-widget-model-koharu` 模型包

3. **配置`_config.yml`**

   ```yml
   
   live2d:
     enable: true
     scriptFrom: local
     pluginRootPath: live2dw/
     pluginJsPath: lib/
     pluginModelPath: assets/
     tagMode: false
     log: false
     model:
       use: live2d-widget-model-koharu
     display:
       position: right
       width: 120
       height: 230
       hOffset: -10
       vOffset: -10
     mobile:
       show: false
   ```

   

## 添加评论

首先，您需要选择一个公共github存储库（已存在或创建一个新的github存储库）用于存储评论。（我的是`blog-talk`）

然后需要创建 **GitHub Application**，如果没有 [点击这里申请](https://github.com/settings/applications/new)。

```bash
Application name： # 应用名称
Homepage URL： # 博客首页地址，对应自己博客地址
Application description ：# 描述
Authorization callback URL：# 网站URL，博客地址，如果是用了第三方域名，则对应第三方域名
```

> 我的Github Application: https://github.com/settings/applications/1513824
>
> ![image-20210328162532211](http://blog.cdn.ionluo.cn/blog/image-20210328162532211.png)



1. **创建gitalk.ejs**

   在你的hexo目录`/theme/yilia/layout/_partial/post/`目录下创建`gitalk.ejs`并写入如下内容：

   ```ejs
   <div id="gitalk-container"></div>
   
   <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
   <script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>
   <script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
   
   <script>
     var gitalk = new Gitalk({
       clientID: '<%=theme.gitalk.clientID%>',
       clientSecret: '<%=theme.gitalk.clientSecret%>',
       repo: '<%=theme.gitalk.repo%>',
       owner: '<%=theme.gitalk.owner%>',
       admin: ['<%=theme.gitalk.admin%>'],
       id: md5(window.location.pathname),
       distractionFreeMode: '<%= theme.gitalk.distractionFreeMode %>'
     })
   
     gitalk.render('gitalk-container')
   </script>
   
   ```

   

2. **修改article.ejs**

   在你的hexo目录`/theme/yilia/layout/_partial/article.ejs`文件中最后一行`“<% } %>”`之前添加如下内容：

   ```ejs
   <!-- begin gittalk -->
   <% if(theme.gitalk.enable && theme.gitalk.distractionFreeMode){ %>
       <%- partial('post/gitalk', {
           key: post.slug,
           title: post.title,
           url: config.url+url_for(post.path)
       }) %>
   <% } %>
   <!-- end gittalk -->
   ```

3. 修改`themes\yilia\source\main.0cf68a.css`， 默认添加样式

   ```css
   #gitalk-container {
     margin: 0 30px;
     position: relative;
     border: 1px solid #ddd;
     border-top: 1px solid #fff;
     border-bottom: 1px solid #fff;
     background: #fff;
     transition: all .2s ease-in;
     padding-right: 7.6923%;
     padding-left: 7.6923%;
     padding-bottom: 36px;
   }
   ```

   

4. **添加配置文件**

   在yilia的配置文件`_config.yml`中`gitment`配置下面添加如下配置文件

   ```yml
   gitalk: 
     enable: true    #用来做启用判断可以不用
     clientID: '7433477a1a82130981e9'    #Github上生成的
     clientSecret: 'xxxxxxxxxxxxxxxx'   #同上
     repo: blog-talk    #评论所在的github project
     owner: Cheerfulion    #github用户名
     admin: Cheerfulion    #可以初始化评论issue的github账户名称
     distractionFreeMode: true
   ```


5. **重启，大功告成**

   ![image-20210328174525366](http://blog.cdn.ionluo.cn/blog/image-20210328174525366.png)



问题记录：

1. **`Error: Validation Failed.`**

   当页面 链接过长 或 存在中文链接，导致整个链接字符串长度超过50时，会造成请求issues接口失败，出现422状态码。（中文链接会自动转码，变得很长，id参数默认取的是链接，长度不能超过50）

   **解决办法：**手动/自动设置id取值，限制长度不超过50。（这里我是通过下面的`URL持久化`实现的）

2. **未找到相关的 [Issues](https://github.com/Cheerfulion/blog-talk/issues) 进行评论**

   每个文章的issues其实是要自己去点击`使用Github登录`才会自动初始化

3. **点击`使用Github登录`后调到首页或者刷新当前页面**

   这个主要就是配置的问题了，首先检查下Github Application的域名配置是否正确。

   其次，检查配置文件`config.yml`中的repo，owner，admin是否正确，注意大多数情况应该保持admin和owner一致，为你的github账号。

4. **gittalk是根据文章的地址进行issue设置并匹配的，所以文章地址不要轻易变更。**



## 侧边栏添加音乐

> 感觉意义不大，现代浏览器基本都会禁止带声音的自动播放，无法作为背景音乐。

**1、网易云音乐外链播放器生成**
登录[网页版网易云音乐](https://music.163.com/)，打开一首歌，点击 “生成外链播放器”。

**2、侧栏添加背景音乐**
打开`/hexo/themes/yilia/layout/_partial/left-col.ejs`文件，把音乐HTML代码粘贴进去，可以添加样式，改变大小，这是我的代码：

```ejs
<nav class="header-nav">
    <div class="social">
        <% for (var i in theme.subnav){ %>
        <a class="<%= i %>" target="_blank" href="<%- url_for(theme.subnav[i]) %>" title="<%= i %>"><i class="icon-<%= i %>"></i></a>
        <%}%>
    </div>
    <!--BEGIN 音乐播放插件-->
    <div style="margin-top:15px;">
        <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=280 height=86 style="max-width: 100%;"
                src="//music.163.com/outchain/player?type=2&id=28815583&auto=1&height=66"></iframe>
    </div>
    <!--END 音乐播放插件-->
</nav>
```





## 添加sitemap

```bash
npm install hexo-generator-sitemap --save
hexo clean
hexo g
```

查看`public`文件夹，可以看到`sitemap.xml`文件。

sitemap的初衷是给搜索引擎看的，为了提高搜索引擎对自己站点的收录效果，我们最好手动到Google和百度等搜索引擎提交sitemap.xml。



## Url持久化(重要)

我们可以发现hexo默认生成的文章地址路径是 `【网站名称／年／月／日／文章名称】`。

这种链接对搜索爬虫是很不友好的，它的url结构超过了三层，太深了。

下面我推荐安装`hexo-abbrlink`插件：

```bash
npm install hexo-abbrlink --save
```

然后修改的`_config.yml`， 替换掉原来的`permalink`

```yml
# permalink: :year/:month/:day/:title/
permalink: posts/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

hexo三连(`hexo clean`, `hexo g`, `hexo s`)之后, 原来长的url就不能访问了，而是类似uuid的形式。其实观察`source/_posts/`里面的文件就会发现，给文章加上了`abbrlink`而已。

![image-20210327182740196](http://blog.cdn.ionluo.cn/blog/image-20210327182751173.png)

## 鼠标点击小红心的效果

在`hexo/themes/yilia/layout/_partial/footer.ejs`文件的最后， 添加以下代码：

```ejs
<!-- START 页面点击小红心-->
<script type="text/javascript">
!function(e,t,a){function r(){for(var e=0;e<s.length;e++)s[e].alpha<=0?(t.body.removeChild(s[e].el),s.splice(e,1)):(s[e].y--,s[e].scale+=.004,s[e].alpha-=.013,s[e].el.style.cssText="left:"+s[e].x+"px;top:"+s[e].y+"px;opacity:"+s[e].alpha+";transform:scale("+s[e].scale+","+s[e].scale+") rotate(45deg);background:"+s[e].color+";z-index:99999");requestAnimationFrame(r)}function n(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),o(e)}}function o(e){var a=t.createElement("div");a.className="heart",s.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:c()}),t.body.appendChild(a)}function i(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function c(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var s=[];e.requestAnimationFrame=e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)},i(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),n(),r()}(window,document);</script>
<!-- START 页面点击小红心-->
```



## 添加 RSS

> [RSS](https://zh.wikipedia.org/wiki/RSS)（简易信息聚合）是一种消息来源格式规范，用以聚合经常发布更新数据的网站，例如博客文章、新闻、音频或视频的网摘。RSS 文件（或称做摘要、网络摘要、或频更新，提供到频道）包含全文或是节录的文字，再加上发布者所订阅之网摘数据和授权的元数据。 ———— 维基百科

关于 RSS 的作用参见：[如何使用 RSS](http://www.ruanyifeng.com/blog/2006/01/rss.html)

- 在 Hexo 根目录打开命令行工具，执行以下命令：

  ```bash
  npm install hexo-generator-feed --sava
  hexo clean
  hexo g
  ```

- 查看 `public` 文件夹，可以看到 **atom.xml** 文件。

- 打开主题配置文件 `/themes/yilia/config.yml`，在 `subnav` 项目下添加 RSS 配置信息：

  ```yml
  # SubNav
  subnav:
    rss: "/atom.xml"
  ```

- 重新生成并构建页面，就可以看到 RSS 的信息了。



## 参考文章

1. [Hexo-Yilia进阶笔记](https://blog.csdn.net/dta0502/article/details/89607895)
2. [Hexo Yilia 主题进阶配置](https://blog.diqigan.cn/posts/hexo-theme-yilia-advanced-settings.html)
3. [Hexo Yilia 高级配置](https://blog.csdn.net/xdfwsl/article/details/102587821)
4. [三步搞定Github Pages自定义域名](https://blog.csdn.net/qq_27346503/article/details/106602720)