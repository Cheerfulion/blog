---
title: vue服务器渲染的实现
description: '-'
tags:
  - WEB开发
  - Web前端
  - SEO
abbrlink: 6034ed99
date: 2021-01-20 00:15:24
---



## 前言

服务器渲染拥有更好的SEO和内容到达时间，但是需要nodejs环境以及更多的服务器端负载。



官方文档：https://ssr.vuejs.org/zh/



如果你倾向于使用提供了平滑开箱即用体验的更高层次解决方案，你应该去尝试使用 [Nuxt.js](https://nuxtjs.org/)。它建立在同等的 Vue 技术栈之上，但抽象出很多模板，并提供了一些额外的功能，例如静态站点生成。但是，如果你需要更直接地控制应用程序的结构，Nuxt.js 并不适合这种使用场景。



>  demo见同级目录中的 `demo`

