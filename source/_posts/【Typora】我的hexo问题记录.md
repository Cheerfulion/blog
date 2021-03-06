---
title: 我的hexo问题记录
description: '-'
tags:
  - hexo
abbrlink: 63cf15be
date: 2021-03-29 23:51:50
disableNunjucks: true
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



## 其他报错

1. `Error [Nunjucks Error]: _posts/xx.md [Line xx, Column xx] expected variable end`

   ![image-20210418095046951](http://blog.cdn.ionluo.cn/blog/image-20210418095046951.png)

   官网有提供说明和解决方法：

   [![image-20210418095436416](http://blog.cdn.ionluo.cn/blog/image-20210418095436416.png)](https://hexo.io/docs/troubleshooting.html)

   **方法一：**给`{{}}`和`{% %}`用一个反引号(`code`)或者三个反引号(`pre`)括起来，或者加上`{% raw %}{% endraw %}`包裹。

   **方法二：**[Front-matter](https://hexo.io/docs/front-matter)加上`disableNunjucks: true`，如下：

   ```markdown
   ---
   title: 【python】django学习
   tags:
     - python
     - django
   abbrlink: 79fab809
   date: 2021-04-15 18:02:23
   disableNunjucks: true
   ---
   ```

   **方法三：**通过Extensions [Disable Nunjucks tags](https://hexo.io/api/renderer#Disable-Nunjucks-tags)

   > 这个方法需要找到我安装的hexo-cli包修改，感觉不能写到项目中，不好做版本控制就放弃了。



## 样式异常

1. 有序列表的标题掉到序号的下一行

   ![image-20210523150314082](http://blog.cdn.ionluo.cn/blog/image-20210523150314082.png)

   **解决方法：**定位到序号这个地方，可以看到是`main.0cf68a.css`的样式文件，找到该文件（themes\yilia\source\main.0cf68a.css）并按照下面修改即可。

   ```css
   .article-entry ol li:before {
       counter-increment: item;
       content: counter(item) ".";
       margin-right: 10px;
       /* 下面是新加的样式 */
       display: inline-block;
       float: left;
   }
   ```

2. 标题序号不正确。

   ![image-20210523150314082](http://blog.cdn.ionluo.cn/blog/image-20210523150314082.png)

   **解决方法：**和上面一样，找到main.css文件，修改下面样式即可。

   ````css
   /* 即把.article-entry ol li:before 改成了 .article-entry ol > li:before */
   .article-entry ol > li:before {
       counter-increment: item;
       content: counter(item) ".";
       margin-right: 10px;
       display: inline-block;
       float: left;
   }
   ````

   

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





## nodejs遍历文件输出

```javascript
// 统计word文件中的产品编号

var fs = require('fs');
// 路径操作模块
const path = require('path');

target_path = String.raw `E:\虚拟机共享文件夹\产品手册`

function readFileList(dir, filesList = []) {
    const files = fs.readdirSync(dir);
    files.forEach((item, index) => {
        var fullPath = path.join(dir, item);
        const stat = fs.statSync(fullPath);
        if (stat.isDirectory()) {      
            readFileList(path.join(dir, item), filesList);  //递归读取文件
        }
        filesList.push(path.join(dir, item))
    });
    return filesList;
}
 
var filesList = [];
readFileList(target_path, filesList);
console.log(filesList);
```

