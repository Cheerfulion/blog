---
title: 我的hexo问题记录
description: 暂无描述！
tags:
  - hexo
abbrlink: 63cf15be
date: 2021-03-28 23:49:11
---



## 生成文件报错

```bash
PS D:\我的代码\my_blog> hexo s
INFO  Validating config
INFO  Start processing
ERROR {
  err: YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key at line 2, column 5:
      tags:
          ^
      at generateError (D:\我的代码\my_blog\node_modules\js-yaml\lib\js-yaml\loader.js:167:10)
......
......
......
reason: 'can not read a block mapping entry; a multiline key may not be an implicit key',
    mark: Mark {
      name: null,
      buffer: 'title: [转载] 解决jenkins点击立即构建没有反应\n' +
        'tags: \n' +
        '    - Jenkins\n' +
        'date: 2021-03-28 18:05:48\n' +
        '\x00',
      position: 36,
      line: 1,
      column: 4
    }
```

原因：标题中带有markdown的格式符号，理论上来说，无论title, tags, date什么头部的内容都不要带上markdown符号，会解析成html。

解决：把`[]`改成`【】`即可。



## 我的nodejs脚本

把本地的一些之前的markdown写的文章复制到hexo博客中

```javascript

```

