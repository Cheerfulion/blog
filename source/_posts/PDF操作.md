---
title: PDF操作
description: '-'
tags:
  - python
abbrlink: 51e665d3
date: 2021-02-23 21:51:33
---



在python中使用PDF的库介绍看这篇文章：https://mp.weixin.qq.com/s/C0j5HZ78gkB9vDfEqlDDPQ

**环境：**

> python==3.6.3
>
> PyMuPDF==1.18.8



## 使用[PyMuPdf](https://pymupdf.readthedocs.io/en/latest/index.html)处理图片

**1.提取图片**

```python
# coding:utf-8
import fitz, os

doc = fitz.open('E:\\files\\123.pdf')
img_count = 0
for page in doc:
    image_list = page.getImageList()
    # print(image_list)
    for img_info in image_list:
        pix = fitz.Pixmap(doc, img_info[0])
        pix.writePNG(os.path.join("img_{}.png".format(img_count)))
        img_count += 1
```

**2.删除PDF中的图片**

```python
# coding:utf-8
import fitz, os

doc = fitz.open('E:\\files\\123.pdf')
print('元数据: ', doc.metadata)
print('文件名称: ', doc.name)
for page in doc:
    image_list = page.getImageList()
    for img_info in image_list:
        doc._deleteObject(img_info[0])

new_name = doc.name.split('/')[-1].split('\\')[-1].replace('.pdf', '_NoImage.pdf')
doc.save(new_name)
```

**2.插入/替换PDF中的图片**

```python
# coding:utf-8
import fitz, os

doc = fitz.open('E:\\files\\123.pdf')

for page in doc:
    # 根据https://github.com/pymupdf/PyMuPDF/issues/367，由于PDF的创建方式不一致，因此特定文档的来源可能不是左上方的标准全局来源。
    # 以下代码段重置了页面的几何形状：
    if not page._isWrapped:
        page.wrapContents()

    # 替换logo（第一张图片）
    image_list = page.getImageList(full=True)
    logo = image_list[0]
    rect = page.getImageBbox(logo)
    img = open("logo.png", "rb").read()
    page.insertImage(rect, stream=img)
    # 如果图片是透明的，可以把原始图片删除，这里是一个不太好解决的问题，删除的是整个文档xref位置的图片，并不是当前页面的该位置图片
    # doc._deleteObject(logo[0])

# 另存为
doc.save('111.pdf')
doc.close()

```

> 除了上面的功能外，PyMuPdf还可以:
>
> [清除元数据信息](https://pymupdf.readthedocs.io/en/latest/document.html#set-metadata-example)
>
> [连接两个文件（包括其目录）](https://pymupdf.readthedocs.io/en/latest/document.html#insert-pdf-examples)
>
>  [将所有页面参考的PDF图像提取到单独的PNG文件中](https://pymupdf.readthedocs.io/en/latest/document.html#other-examples)
>
> [旋转PDF](https://pymupdf.readthedocs.io/en/latest/document.html#other-examples)