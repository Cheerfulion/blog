---
title: 前端SSR
description: 暂无描述！
tags:
  - WEB开发
  - Web前端
  - SEO
abbrlink: 50aae781
date: 2021-01-20 00:15:24
---



## 前言

由于打算使用vue编写项目，但是需要考虑SEO的需求，于是采用SSR服务端渲染的方式提高首页加载速度和SEO优化。

vue官网推荐的SSR方案有两种，[Nuxt.js](https://cn.vuejs.org/v2/guide/ssr.html#Nuxt-js) 和 [Quasar Framework SSR + PWA](https://cn.vuejs.org/v2/guide/ssr.html#Quasar-Framework-SSR-PWA)， 详见：https://cn.vuejs.org/v2/guide/ssr.html。 但是实际上较多受关注的还有Webpack的插件[prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin)，也可以看下面博客学习使用https://www.cnblogs.com/you-uncle/p/13037300.html。

这里由于Nuxt.js是官方推荐，而且有专人维护，更为成熟，因此采用Nuxt作为SSR方案。





## 创建项目

### 使用脚手架创建

环境要求：npm >  5.2.0 

**安装create-nuxt-app脚手架**

```bash
npm i -g create-nuxt-app@2.15.0
```

> 这里不指定版本安装的话是 v3.4.0,  安装选项和官网的文档不一致，因此手动指定下版本。
>
> v3.4.0的安装选项如下图（了解）：
>
> ![1607925118759](assets/前端SSR/1607925118759.png)



**创建项目**

```bash
npx create-nuxt-app ssr_demo[项目名]
```

![1607927184910](assets/前端SSR/1607927184910.png)

> 这里安装完成有报错，如下图：
>
> ![1607927253854](assets/前端SSR/1607927253854.png)
>
> 实际上，这个不是报错，而是当 `eslint` 检查到错误或者警告时，会返回非0的代码，此时就会出现`npm ERR!`。 这里是由于`eslint`修正代码命令修正失败，因为 `--fix` 只能修改那些 fixable 的规则。
>
> https://segmentfault.com/q/1010000014288785/a-1020000014296366





**打开项目本地调试**

```bash
npm run dev
```

![1607929089480](assets/前端SSR/1607929089480.png)

> 中间提示是否愿意发送匿名数据给nuxtJS官方，我选择了no
>
> 第一次运行会报错：
>
> ![1607929170683](assets/前端SSR/1607929170683.png)
>
> 解决方法：项目中编辑 `.eslintrc.js`, 加上`'vue/comment-directive': 'off'`即可
>
> ```javascript
> module.exports = {
>   root: true,
>   env: {
>     browser: true,
>     node: true
>   },
>   parserOptions: {
>     parser: 'babel-eslint'
>   },
>   extends: [
>     '@nuxtjs',
>     'plugin:nuxt/recommended'
>   ],
>   // add your custom rules here
>   rules: {
>     'nuxt/no-cjs-in-config': 'off',
>     'vue/comment-directive': 'off',  // 这个
>   }
> }
> ```
>
> 详情见：https://eslint.vuejs.org/rules/comment-directive.html

### 从头开始新建项目

见：https://www.nuxtjs.cn/guide/installation







