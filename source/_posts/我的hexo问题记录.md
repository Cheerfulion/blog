---
title: 我的hexo问题记录
description: '-'
tags:
  - hexo
abbrlink: 63cf15be
date: 2021-03-29 00:24:31
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

把本地的一些之前的markdown写的文章复制到hexo博客中。

不要经常使用，防止abbrlink被重置。

```javascript
var fs = require('fs');
// 路径操作模块
const path = require('path');
var exec = require('child_process').exec;
// 日期格式化模块
const dateFormat = require('dateformat');

// 需要复制出来的文件夹
form_path = path.join(__dirname, 'origin')
// 复制目标位置
to_path = path.join(__dirname, '..', 'my_blog', 'source', '_posts')
// to_path =  path.join(__dirname, 'copy')

 
function copyToFolder (pathname, stat, filesList) {
    if (path.extname(pathname) !== '.md') return;

    const new_path = path.join(to_path, path.basename(pathname));

    // 这里需要注意，window是两个反斜杆的路径，linux是一个正斜杆路径
    let tags = pathname.split('\\')
    tags = tags.slice(tags.indexOf('origin') + 1, -1)
    let tags_str = ''
    tags.forEach((item) => {
        tags_str += '\n    - ' + item;
    })

    // 文件内容
    let text = fs.readFileSync(pathname).toString();

    // hexo yilia主题貌似不支持头部描述
    text = `---
title: ${path.basename(pathname, path.extname(pathname))}
tags: ${ tags_str }
date: ${ dateFormat(new Date(stat.mtime), "yyyy-mm-dd HH:MM:ss") }
---\n\n\n\n` + text

    fs.writeFileSync(new_path, text);

    filesList.push(pathname)
}

function readFileList(dir, filesList = []) {
    const files = fs.readdirSync(dir);
    files.forEach((item, index) => {
        if (item.includes('asset')) return;

        var fullPath = path.join(dir, item);
        const stat = fs.statSync(fullPath);
        if (stat.isDirectory()) {      
            readFileList(path.join(dir, item), filesList);  //递归读取文件
        } else {  
            copyToFolder(fullPath, stat, filesList);      
        }        
    });
    return filesList;
}
 
var filesList = [];
readFileList(form_path, filesList);
// console.log(filesList);
```

