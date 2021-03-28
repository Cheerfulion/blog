---
title: linux curl wget ab测速
description: 暂无描述！
tags:
  - 运维
  - Linux
abbrlink: 139c023f
date: 2021-03-12 23:04:02
---



> 下面最后加上url即可, 也可以通过测速站点测试https://www.17ce.com/

## CURL测速

```javascript
curl -o /dev/null -w "\n DNS 解析域名的时间\n namelookup:"%{time_namelookup}"\n client和server端建立TCP 连接的时间\n time_connect:"%{time_connect}"\n 从client发出请求；到web的server 响应第一个字节的时间\n time_starttransfer:"%{time_starttransfer}"\n client发出请求；到web的server发送会所有的相应数据的时间\n time_total:"%{time_total}"\n 下载速度 单位 byte/s\n speed_download(byte/s):"%{speed_download}"\n" 
```



### CURL命令测试网站打开速度

```bash
root@web:/var/www/# curl -o /dev/null -s -w %{time_namelookup}:%{time_connect}:%{time_starttransfer}:%{time_total} http://www.baidu.com
# 解析     建立连接  传输   总
0.004129:0.012592:0.024839:0.024881
```

更多curl的用法见：https://www.cnblogs.com/fan-yuan/p/9182902.html



## wget测速

```javascript
wget -O /dev/null
```

### ab测速

```javascript
ab -n 1000 -c 50-k -p ./getByLocation.data -H "referer: https://servicewechat.com/*/devtools/page-frame.html" -T "application/json" -H "Content-Type: application/json" "https://c-mini.*.com/ads/application"
```

### 执行脚本

```javascript
#!/bin/bash
echo "1.拉流接口"
curl -o /dev/null -w "\n DNS 解析域名的时间\n namelookup:"%{time_namelookup}"\n client和server端建立TCP 连接的时间\n time_connect:"%{time_connect}"\n 从client发出请求；到web的server 响应第一个字节的时间\n time_starttransfer:"%{time_starttransfer}"\n client发出请求；到web的server发送会所有的相应数据的时间\n time_total:"%{time_total}"\n 下载速度 单位 byte/s\n speed_download(byte/s):"%{speed_download}"\n"  'https://********.********.********' -H 'sec-ch-ua: nwjs 75' -H 'Sec-Fetch-Mode: cors' -H 'Origin: http://127.0.0.1:56074' -H 'Accept-Encoding: gzip, deflate, br' -H 'Sec-Fetch-Dest: empty' -H 'Connection: keep-alive' -H 'Pragma: no-cache' -H 'User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1 wechatdevtools/1.02.2003250 MicroMessenger/7.0.4 Language/zh_CN webview/' -H 'content-type: application/json;charset=UTF-8' -H 'Accept: */*' -H 'Cache-Control: no-cache' -H 'Referer: https://****.com/********/devtools/page-frame.html' -H 'Sec-Fetch-Site: cross-site' -H 'auth-token: ********' --data-binary '{"action":1,"vid":"********","*****":"********","uid":"********","appId":"********","**":"********","***":"********","**":[1]}' --compressed

echo "2.搜索接口"
curl -o /dev/null -w "\n DNS 解析域名的时间\n namelookup:"%{time_namelookup}"\n client和server端建立TCP 连接的时间\n time_connect:"%{time_connect}"\n 从client发出请求；到web的server 响应第一个字节的时间\n time_starttransfer:"%{time_starttransfer}"\n client发出请求；到web的server发送会所有的相应数据的时间\n time_total:"%{time_total}"\n 下载速度 单位 byte/s\n speed_download(byte/s):"%{speed_download}"\n"  'https://********.********.********' -H 'sec-ch-ua: nwjs 75' -H 'Sec-Fetch-Mode: cors' -H 'Origin: http://127.0.0.1:56074' -H 'Accept-Encoding: gzip, deflate, br' -H 'Sec-Fetch-Dest: empty' -H 'Connection: keep-alive' -H 'Pragma: no-cache' -H 'User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1 wechatdevtools/1.02.2003250 MicroMessenger/7.0.4 Language/zh_CN webview/' -H 'content-type: application/json;charset=UTF-8' -H 'Accept: */*' -H 'Cache-Control: no-cache' -H 'Referer: https://****.com/********/devtools/page-frame.html' -H 'Sec-Fetch-Site: cross-site' -H 'auth-token: ********' --data-binary '{"action":1,"vid":"********","*****":"********","uid":"********","appId":"********","**":"********","***":"********","**":[1]}' --compressed

echo "3.资源地址"
wget -O /dev/null https://*.mp4
```

### 输出

```bash
1.拉流接口
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   339    0   162  100   177    572    625 --:--:-- --:--:-- --:--:--   627

 DNS 解析域名的时间
 namelookup:0.004
 client和server端建立TCP 连接的时间
 time_connect:0.035
 从client发出请求；到web的server 响应第一个字节的时间
 time_starttransfer:0.283
 client发出请求；到web的server发送会所有的相应数据的时间
 time_total:0.283
 下载速度 单位 byte/s
 speed_download(byte/s):572.000
2.搜索接口
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5899    0  5630  100   269  16064    767 --:--:-- --:--:-- --:--:-- 16039

 DNS 解析域名的时间
 namelookup:0.000
 client和server端建立TCP 连接的时间
 time_connect:0.035
 从client发出请求；到web的server 响应第一个字节的时间
 time_starttransfer:0.350
 client发出请求；到web的server发送会所有的相应数据的时间
 time_total:0.350
 下载速度 单位 byte/s
 speed_download(byte/s):16064.000
3.资源地址
--2020-04-22 11:32:12--  https://*.mp4
Resolving *.com)... *.*.*.*, *.*.*.*,*.*.*.*
Connecting to *.com (*.com)|*.*.*.*|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16178920 (15M) [video/mp4]
Saving to: '/dev/null'

100%[===============================================================================================
```





