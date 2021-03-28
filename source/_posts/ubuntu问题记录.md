---
title: ubuntu问题记录
description: 暂无描述！
tags:
  - 运维
  - Linux
abbrlink: d534665a
date: 2021-03-12 23:00:31
---



## The repository 'http://ppa.launchpad.net/chris-lea/node.js/ubuntu bionic Release' does not have a Release file.

解决：

```bash
sudo add-apt-repository --remove ppa:/chris-lea/node.js 
sudo apt-get update
```

