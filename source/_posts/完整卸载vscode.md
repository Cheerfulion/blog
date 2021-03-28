---
title: 完整卸载vscode
description: 暂无描述！
tags:
  - IDE
  - VSCode
abbrlink: 859b532b
date: 2021-03-16 23:07:29
---



这两天VSCODE老是出现各种问题，窗口打开出现故障，eslint插件无法启动，终端无法启动等等。



于是重装vscode，发现各种配置和插件还存在，无法完全卸载。于是参考下面文章卸载。

> 温习提示：为了保险，删除文件前请先备份

https://blog.csdn.net/jpch89/article/details/89789247



## 解决方案

> **注意**：以下步骤需要在执行 `VSCode` 自带卸载程序之后执行。

- `win + r` 打开运行
- `%appdata%` 回车
- 删除 `Code` 和 `Visual Studio Code` 文件夹
- 地址栏输入 `%userprofile%` 回车
- 删除 `.vscode` 文件夹



ok, 终端和ESLint正常了。
