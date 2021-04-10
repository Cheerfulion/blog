---
title: 运行可执行文件
description: '-'
tags:
  - 运维
  - Linux
abbrlink: '14433368'
date: 2021-01-08 23:43:28
---



打开一个可执行文件，如果你的可执行文件文件名为`wkhtmltopdf-amd64`，则命令为  `./wkhtmltopdf-amd64`



当然绝对路径也是可以运行的，错误示例是直接运行，如下：

```bash
cd /var/www/bin/wkhtmltopdf/0.12.3
# 错误
wkhtmltopdf-amd64 --version
# 正确
./wkhtmltopdf-amd64 --version
# 正确
/var/www/bin/wkhtmltopdf/0.12.3/wkhtmltopdf-amd64 --version
```

