---
title: 报错
description: '-'
tags:
  - Git
abbrlink: caf197e9
date: 2021-03-14 11:26:24
---



### fatal: unable to access 'https://github.com/getsentry/onpremise.git/': gnutls_handshake() failed: The TLS connection was non-properly terminated.

原因查找：

我这里发现是这个仓库的链接国内无法访问，因此要设置git代理来访问（需要搭梯子）。

```bash
#设置socks5代理或者http代理（任选一种）
git config --global http.proxy 'socks5://172.20.0.62:8388'
git config --global https.proxy 'socks5://172.20.0.62:8388'
git config --global http.proxy 'http://127.0.0.1:45077'
git config --global https.proxy 'http://127.0.0.1:45077'

# 这时候就可以克隆了

# 如果之后要删除代理，使用如下命令
git config --global --unset http.proxy
git config --global --unset https.proxy
```

