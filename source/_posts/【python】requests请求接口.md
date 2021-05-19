---
title: 【python】requests请求接口
tags: python
abbrlink: 5dfdd5bc
date: 2021-05-14 13:53:21
---



## post请求数据格式类型

- application/json：是JSON格式提交的一种识别方式。
- application/x-www-form-urlencoded ： 这是form表单提交的时候的表示方式
- multipart/form-data
  这又是一个常见的 POST 数据提交的方式。我们使用表单上传文件时，必须让 form 的 enctyped 等于这个值
- text/xml
  它是一种使用 HTTP 作为传输协议，XML 作为编码方式的远程调用规范



## 请求接口准备

```bash
# python2.7可以使用requests==2.10.0
pip install requests
pip install fake-useragent
```



## 实际操作

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import requests
from fake_useragent import UserAgent

ua = UserAgent()

headers = {
    'Host': 'zoom.us',
    'User-Agent': ua.random,
    'Accept': 'application/json, text/javascript, */*; q=0.01',
    'Accept-Language': 'zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2',
    'Accept-Encoding': 'gzip, deflate, br',
    'Referer': 'https://zoom.us/webinar/register/WN_hZH9x5yQT1SVFT4vj54s5Q',
    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
    'X-Requested-With': 'XMLHttpRequest, XMLHttpRequest, OWASP CSRFGuard Project',
    'ZOOM-CSRFTOKEN': 'H7HW-2OGY-EL3Z-WQNN-1GDQ-GKXA-VJT0-TOZV',
    'Content-Length': 564,
    'Origin': 'https://zoom.us',
    'Connection': 'keep-alive',
    # 'Cookie': '_zm_sa_si_none=1; cred=2D2920BB1DB5E82D1034FD52F67FD425; _zm_page_auth=aw1_c_dnMtHuZqQBG2tLjvtf310A; _zm_ssid=aw1_c_Sz8vgjduQhmdjbuwgJDkSw; _zm_cta=8aQ-shBBR4qVkfRVo0hAmw; _zm_ctaid=bKKn1n1DTP2k3qDY-9a1Xw.1620969620559.2899396dec8fe5347730c6e9b5b46e3f; _zm_chtaid=641; _zm_lang=zh-CN; _zm_csp_script_nonce=ZoMa-6ujQtCe_8vssYvfgQ; _zm_currency=CNY; _zm_mtk_guid=ffe7a73dbf0b46d9be6977c6b7e4ba9b; _zm_cdn_blocked=unlog_unblk',
    'TE': 'Trailers',
}

cookies = {
    '_zm_sa_si_none': 1,
    'cred': '2D2920BB1DB5E82D1034FD52F67FD425',
    '_zm_page_auth': 'aw1_c_dnMtHuZqQBG2tLjvtf310A',
    '_zm_ssid': 'aw1_c_Sz8vgjduQhmdjbuwgJDkSw',
    '_zm_cta': '8aQ-shBBR4qVkfRVo0hAmw',
    '_zm_ctaid': 'bKKn1n1DTP2k3qDY-9a1Xw.1620969620559.2899396dec8fe5347730c6e9b5b46e3f',
    '_zm_chtaid': 641,
    '_zm_lang': 'zh-CN',
    '_zm_csp_script_nonce': 'ZoMa-6ujQtCe_8vssYvfgQ',
    '_zm_currency': 'CNY',
    '_zm_mtk_guid': 'ffe7a73dbf0b46d9be6977c6b7e4ba9b',
    '_zm_cdn_blocked': 'unlog_unblk'
}

datas = {
    "first_name": "ion",
    "last_name": "luo",
    "email": "ionluo@xxx.com",
    "questions": "[{\"name\":\"first_name\",\"value\":\"ion\"},{\"name\":\"last_name\",\"value\":\"luo\"},{\"name\":\"email\",\"value\":\"ionluo@xxx.com\"},{\"name\":\"confirm_email\",\"value\":\"ionluo@xxx.com\"}]",
    "meeting_id": "",
    "play_id": "",
    "recordAction": "",
    "custom_questions": "[]",
    "recording_id": "",
    "timezone_id": "Asia/Tokyo",
    "sourceId": "",
    "fee": "",
    "unit": "",
    "occurrence_times": "",
    "recaptcha": "",
    "scc": "",
    "uri": "/webinar/register/WN_hZH9x5yQT1SVFT4vj54s5Q",
    "fm": ""
}
datas = str(datas).replace("+", "%2B")
datas = datas.encode("utf-8")

session = requests.session()
url = 'https://zoom.us/webinar/register/save/tJIkf-2ppj4tGNNJZ45a3qt1ML1Ud-d80oLu'

# 如果就是发送json数据，可以修改data为json
response = requests.post(url, data=datas, headers=headers, cookies=cookies, timeout=30)
if response.status_code in [200, 201, 202]:
    # response.content response.json()
    print(response.text)
else:
    print('error!')
```

> 上面只是一个例子，实际是无法请求成功的，发现这里有两层安全校验——谷歌验证码和CRSF，不太懂怎么破解，有时间研究下补充。