---
title: html，word，pdf的转化
description: '-'
tags:
  - 工作遇到的问题
abbrlink: '95230035'
date: 2021-04-02 21:04:52
---



## html→word

### 方式1：html-docx-js

**1、npm安装**

```bash
$ npm install --save html-docx-js
$ npm install --save file-saver
```

**2、引入**

```bash
import htmlDocx from 'html-docx-js/dist/html-docx';
import saveAs from 'file-saver';
```

**3、点击事件实现导出**

```html
<template>
  <div class="about">
    <button @click="this.exportClick">an</button>
  </div>
</template>

<script>
import htmlDocx from 'html-docx-js/dist/html-docx';
import saveAs from 'file-saver';
export default {
  methods: {
    exportClick() {
      var content = ` <h1>This is an about page</h1>
      <h2>This is an about page</h2>`
      var page = '<!DOCTYPE html><html><head><meta charset="UTF-8"></head><body>' + content + '</body></html>'
      var converted = htmlDocx.asBlob(page);
      // 用 FielSaver.js里的保存方法 进行输出
      saveAs(converted, 'test.docx');
    }
  }
}
</script>
```

**4、带预览的下载**

```html
<!doctype html>
<html>

<head>
    <meta charset="utf-8">
    <title>HTML-DOCX test</title>
    <link rel="stylesheet" href="http://blog.cdn.ionluo.cn/lib/bootstrap/dist/css/bootstrap.min.css">
    <script src="http://blog.cdn.ionluo.cn/lib/tinymce/tinymce.min.js"></script>
    <script src="http://blog.cdn.ionluo.cn/lib/html-docx-js/test/vendor/FileSaver.js"></script>
    <script src="http://blog.cdn.ionluo.cn/lib/html-docx-js/dist/html-docx.js"></script>
    <!--    https://github.com/evidenceprime/html-docx-js -->
</head>

<body>
    <div class="container">
        <p>Enter/paste your document here:</p>

        <textarea class="form-control" id="content" rows="10">
            <p>We all live in a yellow submarine, yellow submarine, yellow submarine, yellow submarine</p>
            <p>Images can also be exported if you source them as base64 DATA URI.</p>
            <img src="./lib/html-docx-js/test/cat.jpg">
            <!-- <img src="./lib/html-docx-js/test/cat.jpg"> -->
        </textarea>

        <div class="page-orientation">
            <span>Page orientation:</span>
            <label><input type="radio" name="orientation" value="portrait" checked>Portrait</label>
            <label><input type="radio" name="orientation" value="landscape">Landscape</label>
        </div>

        <button id="convert">Convert</button>
        <div id="download-area"></div>
    </div>

    <script>
        tinymce.init({
            selector: '#content',
            plugins: [
                "advlist autolink lists link image charmap print preview anchor",
                "searchreplace visualblocks code fullscreen fullpage",
                "insertdatetime media table contextmenu paste"
            ],
            toolbar: "insertfile undo redo | styleselect | bold italic | " +
                "alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | " +
                "link image"
        });

        // 异步获取html文档
        // var Ajax = {
        //     get: function (url, callback) {
        //         // XMLHttpRequest对象用于在后台与服务器交换数据
        //         var xhr = new XMLHttpRequest();
        //         xhr.open('GET', url, false);
        //         xhr.onreadystatechange = function () {
        //             // readyState == 4说明请求已完成
        //             if (xhr.readyState == 4) {
        //                 if (xhr.status == 200 || xhr.status == 304) {
        //                     callback(xhr.responseText);
        //                 }
        //             }
        //         }
        //         xhr.send();
        //     },

        //     // data应为'a=a1&b=b1'这种字符串格式，在jq里如果data为对象会自动将对象转成这种字符串格式
        //     post: function (url, data, callback) {
        //         var xhr = new XMLHttpRequest();
        //         xhr.open('POST', url, false);
        //         // 添加http头，发送信息至服务器时内容编码类型
        //         xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        //         xhr.onreadystatechange = function () {
        //             if (xhr.readyState == 4) {
        //                 if (xhr.status == 200 || xhr.status == 304) {
        //                     // console.log(xhr.responseText);
        //                     callback(xhr.responseText);
        //                 }
        //             }
        //         }
        //         xhr.send(data);
        //     }
        // };
        // Ajax.get('xxxx', function (res) {
        // setTimeout(function () {
        //     var tinymceDocument = tinymce.get('content').getDoc();
        //     tinymceDocument.documentElement.innerHTML = res;
        // }, 1000)
        // var converted = htmlDocx.asBlob(res);
        // saveAs(converted, 'test.docx');
        // });

        document.getElementById('convert').addEventListener('click', function (e) {
            // e.preventDefault();
            convertImagesToBase64()
            // for demo purposes only we are using below workaround with getDoc() and manual
            // HTML string preparation instead of simple calling the .getContent(). Becasue
            // .getContent() returns HTML string of the original document and not a modified
            // one whereas getDoc() returns realtime document - exactly what we need.
            var contentDocument = tinymce.get('content').getDoc();
            var content = '<!DOCTYPE html>' + contentDocument.documentElement.outerHTML;

            var orientation = document.querySelector('.page-orientation input:checked').value;
            var converted = htmlDocx.asBlob(content, { orientation: orientation });

            // 自动下载
            saveAs(converted, 'document.docx');

            // 手动下载
            var link = document.createElement('a');
            link.href = URL.createObjectURL(converted);
            link.download = 'document.docx';
            link.appendChild(
                document.createTextNode('Click here if your download has not started automatically'));
            var downloadArea = document.getElementById('download-area');
            downloadArea.innerHTML = '';
            downloadArea.appendChild(link);
        });

        function convertImagesToBase64() {
            contentDocument = tinymce.get('content').getDoc();
            var regularImages = contentDocument.querySelectorAll("img");
            var canvas = document.createElement('canvas');
            var ctx = canvas.getContext('2d');
            [].forEach.call(regularImages, function (imgElement) {
                // preparing canvas for drawing
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                canvas.width = imgElement.width;
                canvas.height = imgElement.height;
                // 这个可以阻止画布被污染问题
                imgElement.setAttribute('crossorigin', 'Anonymous')

                ctx.drawImage(imgElement, 0, 0);
                // by default toDataURL() produces png image, but you can also export to jpeg
                // checkout function's documentation for more details
                var dataURL = canvas.toDataURL();
                imgElement.setAttribute('src', dataURL);
            })
            canvas.remove();
        }
    </script>
</body>
</html>
```

> 关于canvas的踩坑，推荐阅读：https://juejin.cn/post/6844903855214297102



**效果如图：**

![image-20210401175617323](http://blog.cdn.ionluo.cn/blog/image-20210401175617323.png)



### 方式2：pandoc

**ubuntu安装：**

```bash
sudo apt install pandoc
pip3 install pypandoc
```

> 官网：[pandoc](https://pandoc.org/MANUAL.html) 通用文档转换器
>
> 更多：[【解锁】Pandoc——Pandoc安装、使用、快速上手](https://blog.csdn.net/xk_xx/article/details/104179256/)
>
> 官网： [pypandoc 1.5](https://pypi.org/project/pypandoc/)

**示例代码：**

```python
# !/usr/bin/env python
# # -*- coding: utf-8 -*-
"""
    @ Convert Html To Docx
"""

import pypandoc

html = """
<h3>This is a title</h3>
<p><img src="http://placehold.it/150x150" alt="I go below the image as a caption" class="img-responsive center-block" /></p>
<p><i>This is <b>some</b> text</i> in a <a href="http://google.com">paragraph</a></p>
<ul>
    <li>Boo! I am a <b>list</b></li>
</ul>
"""
# 将网页直接转换成docx
output = pypandoc.convert_file('1.html', 'docx', outputfile="file1.docx")
# 将 html 代码转化成docx
output = pypandoc.convert_text(html, 'docx', 'html', outputfile="file1.docx")
```

> **扩展阅读：**
>
> [MarkDown使用pandoc转PDF和Word文档](https://blog.csdn.net/liuguangrong/article/details/52858595)
>
> **注意：**该插件不支持pdf→word



**问题1：`Stack space overflow: current size xxx bytes.` **

```bash
# Stack space overflow: current size 33624 bytes.
# Use `+RTS -Ksize -RTS' to increase it.
# 暂时只知道pandoc的命令可以这样子，但是pypandoc不知道怎么设置
pandoc +RTS -K100000000 -RTS test.html -o test.doc
```



**问题2：效果不好，样式无法设置，除了文字的样式还算ok，其他都不太行。 **

![image-20210401174510390](http://blog.cdn.ionluo.cn/blog/image-20210401174510390.png)


### 方式3：LibreOffice

```python
# Ubuntu安装libreoffice（windows上见https://blog.csdn.net/weixin_34032779/article/details/86022062）
# sudo apt-get install libreoffice

# 设置中文界面
# sudo apt-get install libreoffice-l10n-zh-cn libreoffice-help-zh-cn

# python源码
import os
import subprocess

filename = os.path.join(os.path.abspath('.'), 'index.html')
subprocess.call('lowriter --invisible --convert-to doc --outdir "{}" "{}"'.format('.', filename), shell=True)
```

**效果：**

![image-20210402134427720](http://blog.cdn.ionluo.cn/blog/image-20210402134427720.png)



## word→pdf

### 方式1：Office+win32com

> 该方式只适用于window系统，且有安装office（测试WPS也可以）。

```python
# -*- encoding: utf-8 -*-
import os
# pip install win32com
from win32com import client


def doc2pdf(doc_name, pdf_name):
    """
    :word文件转pdf
    :param doc_name word文件名称
    :param pdf_name 转换后pdf文件名称
    """

    pdf_path = pdf_name.split(os.sep)[0:-1]
    pdf_path = os.sep.join(pdf_path)
    print('pdf_path', pdf_path)
    if not os.path.exists(pdf_path):
        os.makedirs(pdf_path)

    w = client.DispatchEx("Word.Application")
    w.Visible = 0  # 0:后台运行 1:前台运行(可见)
    w.DisplayAlerts = 0  # 不显示，不警告
    if os.path.exists(pdf_name):
        os.remove(pdf_name)
    word = w.Documents.Open(doc_name, ReadOnly=1)
    word.SaveAs(pdf_name, FileFormat=17)
    word.Close()
    try:
        w.Documents.Close()
        w.Quit()
    except Exception as e:
        print('error', e)


# 遍历获取文件夹下的所有word文件(返回的结果是相对于root的相对路径)
def dfs_get_word_file(cwd, result=[], root=None):
    root = root or cwd
    files = os.listdir(cwd)
    for file in files:
        sub_dir = os.path.join(cwd, file)
        if os.path.isdir(sub_dir):
            dfs_get_word_file(sub_dir, result, root)
        elif os.path.splitext(sub_dir)[-1] in ['.docx', '.doc']:
            result.append(sub_dir.split(root+os.sep)[-1])
    return result


if __name__ == '__main__':
    # word_path = os.path.join(os.path.abspath(__file__), '..', 'word')
    # pdf_path = os.path.join(os.path.abspath(__file__), '..', 'pdf')
    word_path = r'E:\虚拟机共享文件夹\产品手册'
    pdf_path = r'E:\虚拟机共享文件夹\产品手册PDF'

    word_files = dfs_get_word_file(word_path, [])
    for file in word_files:
        doc_name = os.path.join(word_path, file)
        pdf_name = os.path.join(pdf_path, file.split('.')[0] + '.pdf')
        doc2pdf(doc_name, pdf_name)
    print('done.')
```



## pdf→word

### 方式1：pdfminer+python-docx

> 原理是通过pdfminer提取文字，通过python-docx存入docx。因此仅仅适合带文字的PDF。

**依赖插件：**

```bash
# vim requirements.txt
attrs==17.4.0
lxml==4.1.1
pdfminer3k==1.3.2
pluggy==0.6.0
ply==3.11
py==1.5.2
pytest==3.4.1
python-docx==0.8.6
six==1.11.0

# 安装插件(这里我用的是python3的包管理器, 如果你的就是python3，用pip即可)
pip3 install requirements.txt
```

**python代码：**

```python
# # -*- coding: utf-8 -*-
"""
    @ Covert PDF to Word
"""

import os
from io import StringIO
from io import open
from concurrent.futures import ProcessPoolExecutor
from pdfminer.pdfinterp import PDFResourceManager
from pdfminer.pdfinterp import process_pdf
from pdfminer.converter import TextConverter
from pdfminer.layout import LAParams
from docx import Document


PROJECT_PATH = os.path.normpath(os.path.dirname(os.path.abspath(__file__)))

pdf_folder = os.path.join(PROJECT_PATH, 'pdf')
word_folder = os.path.join(PROJECT_PATH, 'word')


def read_from_pdf(file_path):
    with open(file_path, 'rb') as file:
        resource_manager = PDFResourceManager()
        return_str = StringIO()
        lap_params = LAParams()

        device = TextConverter(
            resource_manager,
            return_str,
            laparams=lap_params)
        process_pdf(resource_manager, device, file)
        device.close()

        content = return_str.getvalue()
        return_str.close()
        return content


def save_text_to_word(content, file_path):
    doc = Document()
    for line in content.split('\n'):
        paragraph = doc.add_paragraph()
        paragraph.add_run(remove_control_characters(line))
    doc.save(file_path)


def remove_control_characters(content):
    mpa = dict.fromkeys(range(32))
    return content.translate(mpa)


# 将两个函数封装起来
def pdf_to_word(pdf_file_path, word_file_path):
    # 这里使用ProcessPoolExecutor会遇到错误并不输出，所以我们要自己捕获输出下
    try:
        content = read_from_pdf(pdf_file_path)
        save_text_to_word(content, word_file_path)
    except Exception as e:
        print('error', e)

if __name__ == '__main__':
    tasks = []
    with ProcessPoolExecutor(max_workers=5) as executor:
        for file in os.listdir(pdf_folder):
            extension_name = os.path.splitext(file)[1]
            if extension_name != '.pdf':
                continue
            file_name = os.path.splitext(file)[0]
            pdf_file = pdf_folder + '/' + file
            word_file = word_folder + '/' + file_name + '.docx'
            print('正在处理: ', file)
            result = executor.submit(pdf_to_word, pdf_file, word_file)
            tasks.append(result)

    while True:
        exit_flag = True
        for task in tasks:
            if not task.done():
                exit_flag = False
        if exit_flag:
            print('完成')
            exit(0)
```



**效果：**

![image-20210401174718920](http://blog.cdn.ionluo.cn/blog/image-20210401174718920.png)

### 方式2：LibreOffice

```python
# Ubuntu安装libreoffice（windows上见https://blog.csdn.net/weixin_34032779/article/details/86022062）
# sudo apt-get install libreoffice

# 设置中文界面
# sudo apt-get install libreoffice-l10n-zh-cn libreoffice-help-zh-cn

# python源码
import os
import subprocess

for top, dirs, files in os.walk('/my/pdf/folder'):
    for filename in files:
        if filename.endswith('.pdf'):
            abspath = os.path.join(top, filename)
            subprocess.call('lowriter --invisible --convert-to doc "{}"'.format(abspath), shell=True)
```



**问题：无法执行，缺少过滤器，截图和说明见：**

![image-20210402100314407](http://blog.cdn.ionluo.cn/blog/image-20210402100314407.png)

原因分析：

```bash
This is not a bug. Using --convert-to includes normal opening a document
detecting the appropriate module; and then exporting from that module's data to
a chosen format, *if such export filter exists for that module*.

As you possibly know, using LibreOffice GUI to handle PDFs opens them in Draw.
You need to choose Writer's import filter explicitly to open PDF in Writer. And
only from Writer you can export to a text document format like DOCX.

That is also true for CLI. The command line from comment 0 opens the PDF in
Draw component, and then fails to find a suitable export filter.

You need to define import filter explicitly, like here:

> soffice --infilter="writer_pdf_import" --convert-to docx file.pdf

# 上面的soffice是window下的命令，linux下的是lowriter，更多关于libreoffice的用法见：
# https://blog.csdn.net/weixin_34032779/article/details/86022062
```

纠正代码：

```python
# # -*- coding: utf-8 -*-
"""
    @ Covert PDF to Word
"""
import os
import subprocess

for top, dirs, files in os.walk('/my/pdf/folder'):
    for filename in files:
        if filename.endswith('.pdf'):
            abspath = os.path.join(top, filename)
            subprocess.call('lowriter --invisible --infilter="writer_pdf_import" --convert-to doc --outdir "{}" "{}"'.format('/my/word/folder', abspath), shell=True)
```

> 参考：https://www.mail-archive.com/libreoffice-bugs@lists.freedesktop.org/msg559573.html



**总结：**

效果是比较好的，但是转docx还是会出现格式混乱的问题。同时在wps中页面有一定的偏移，在桌面版Ubuntu的LibreOffice Writer中打开会卡死，原因未知。

![image-20210402102706439](http://blog.cdn.ionluo.cn/blog/image-20210402102706439.png)





> **wps中页面偏移解决办法：**
>
> 页面偏移主要是WPS默认页边距不为0导致的，而且由于转化的word中有分页符，所以需要在页面布局→页面设置→页边距→填入页边距值，选择应用于“整篇文档”即可。
>
> ![image-20210414092014241](http://blog.cdn.ionluo.cn/blog/image-20210414092014241.png)
>
> 参考：https://jingyan.baidu.com/article/2c8c281dea9bea4109252a54.html
>
> 
>
> **LibreOffice Writer打开卡死解决办法：**
>
> 暂无
>
> 尝试了下面的方法，但是还是卡
>
> https://linux.zone/2997





### 方式3：Office+win32com

> 该方式只适用于window系统，且有安装office。

```python
# 安装win32com(window外的系统会报错)
# python -m pip install pypiwin32

import win32com.client
import os

word = win32com.client.Dispatch("Word.Application")
word.visible = 0

pdf_path = os.path.join(os.path.abspath('.'), 'pdf')
word_path = os.path.join(os.path.abspath('.'), 'word')

for top, dirs, files in os.walk(pdf_path):
    for filename in files:
        if not filename.endswith('.pdf'):
            continue

        abspath = os.path.join(top, filename)
        wb = word.Documents.Open(abspath)
        wb.SaveAs2(os.path.join(word_path, filename[0:-4] + ".docx"), FileFormat=16)  # file format for docx
        wb.Close()
# word.Quit()
```





**问题1：报错`AttributeError: 'NoneType' object has no attribute 'SaveAs2'`**

由于我这里安装的是WPS，所以wb对象其实是WPS的，没有这些操作。

参考：https://www.imooc.com/qadetail/325176



**效果：**

由于我没有安装Office，而且实际上我需要在服务器使用，所以就没有测试效果了。





### 方式4：GroupDocs Python SDK

> 注意：由于是上传到第三方，所以缺点是很明显的，对于敏感文件不宜轻易上传。

码字中...



> 还有一个类似的方式，效果也是不错，但是有限制，毕竟作者要恰饭，哈！
>
> https://mp.weixin.qq.com/s/Hu6aheMECl6GQNWMudNj6w



## word→html

### 方式1：pydocx

```python
# # -*- coding: utf-8 -*-
"""
    @ Covert Word to Html
"""

from pydocx import PyDocX
html = PyDocX.to_html("./test.docx")
f = open("./test.html", 'w', encoding="utf-8")
f.write(html)
f.close()
```





## 总结

**对于pdf, word, html的互转，万精油的方法是LibreOffice和win32com，同时效果也是最好的，但是win32com注意只能适用于window系统。**

**对于html转PDF，推荐使用wkhtmltopdf，还可以使用puppeteer（未测试效果）。**



> 2021.4.2更新，上面html转换后样式缺失的问题估计是不支持外链加载，可以使用[juice](https://github.com/Automattic/juice)（推荐）或者 [html-packer](https://github.com/rozbo/html-packer)插件处理。这两个插件都可以实现把html的外部资源合并到一个html中。



## 参考

[html导出docx文件](https://blog.csdn.net/weixin_46115860/article/details/107731454)

[Python实现PDF转Word](https://zhuanlan.zhihu.com/p/74781263?from_voters_page=true)

[Convert PDF to DOC (Python/Bash)](https://stackoverflow.com/questions/26358281/convert-pdf-to-doc-python-bash)

[How to Convert PDF to Word With Python: A Step-by-Step Guide](https://www.talkhelper.com/how-to-convert-pdf-to-word-with-python)

[在前端如何玩转 Word 文档](https://segmentfault.com/a/1190000023212724)

[在前端 Word 还能这样玩](https://segmentfault.com/a/1190000021111457)

