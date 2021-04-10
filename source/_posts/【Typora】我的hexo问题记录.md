---
title: 我的hexo问题记录
description: '-'
tags:
  - hexo
abbrlink: 63cf15be
date: 2021-03-29 23:51:50
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
// markdown解析成html "marked": "^1.2.7",
const marked = require('marked')
// 日期格式化模块 "dateformat": "^3.0.3",
const dateFormat = require('dateformat');
// DOM操作 "cheerio": "^1.0.0-rc.5",
const cheerio = require('cheerio');

// 需要复制出来的文件夹
form_path = path.join(__dirname, 'origin')
// 复制目标位置
to_path = path.join(__dirname, '..', 'my_blog', 'source', '_posts')
// to_path =  path.join(__dirname, 'copy')

 
function copyToFolder (pathname, stat, filesList) {
    if (pathname.indexOf('private-') > -1) return;
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
    // // markdown文件解析成html
    // let html = marked(text);
    // $ = cheerio.load(html);
    // let description = $('p').text().substr(0, 300).replace(/>-/g, '').replace(/:/g, '').replace(/\n/g, '') || '';

    // hexo头部描述(description暂时不太清楚其规则，先不管，空着)
    text = `---
title: ${path.basename(pathname, path.extname(pathname))}
description: '-'
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

