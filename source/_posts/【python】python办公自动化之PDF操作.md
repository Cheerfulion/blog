---
title: 【python】python办公自动化之PDF操作
tags:
  - python
abbrlink: 407ff8b6
date: 2021-05-26 09:49:53
---



## 前言

待梳理



## 源码

```python
# coding:utf-8

# 这里用模板文件替换掉原来的PDF的一些样式，原来PDF的其他不改动

import fitz, os
from PIL import Image
from PyPDF2 import PdfFileReader, PdfFileWriter

old_path = 'E:\\project\\EDM\\demo\\python\\pdf\\assets\\细胞产品.pdf'
new_path = 'E:\\project\\EDM\\demo\\python\\pdf\\assets\\细胞产品模板.pdf'

# PyPDF2 每次修改PDF会影响到整个原来文档的信息，放弃使用
# watermark_obj = PdfFileReader(new_path)
# watermark_page = watermark_obj.getPage(1)
#
# pdf_reader = PdfFileReader(old_path)
# pdf_writer = PdfFileWriter()
#
# # 给所有页面添加水印
# for i in range(pdf_reader.getNumPages()):
#     page = pdf_reader.getPage(i)
#     if i != 0:
#         page.mergePage(watermark_page)
#     pdf_writer.addPage(page)
#
# with open('111.pdf', 'wb') as out:
#     pdf_writer.write(out)

doc = fitz.open(old_path)
template = fitz.open(new_path)

logo=None

for page in doc:
    # 根据https://github.com/pymupdf/PyMuPDF/issues/367，由于PDF的创建方式不一致，因此特定文档的来源可能不是左上方的标准全局来源。
    # 以下代码段重置了页面的几何形状：
    if not page._isWrapped:
        page.wrapContents()

    if page.number == 0:
        # 第一页是封面，特殊处理
        # 1. 提取原来封面的标题和货号
        # 2. 删除原来的封面
        # 3. 新封面标题货号替换后插入文档
        # print (page.getText())
        # text_encode = page.getText().encode("utf8")

        doc.insertPDF(template, to_page=0)
        doc.move_page(len(doc) - 1, 0)
        name = template.loadPage(0).searchFor("产品名称")
        number = template.loadPage(0).searchFor("产品编号")
        name = name[0]
        number = number[0]
        # print("name", name[0])
        # print("number", number[0])
        # doc._deleteObject(int(name[0]))
        # doc._deleteObject(int(number[0]))
        # point = fitz.Point(50, 100)
        # doc.loadPage(0).insertText(point, text, fontsize=11,
        #                            rotate=0,
        #                            color=(0, 0, 1),
        #                            overlay=True)
        doc.deletePageRange(1, 1)
    else:
        # 插入页脚图片
        rect = fitz.Rect(0, 0, 614, 1670)
        # im = Image.open("图片2.png")
        # (x, y) = im.size
        # x_s = 614
        # y_s = 12
        # out = im.resize((x_s, y_s), Image.ANTIALIAS)
        # out.save("图片3.png")
        img = open("图片3.png", "rb").read()
        page.insertImage(rect, stream=img)

        # 替换logo（第一张图片）
        # image_list = page.getImageList(full=True)
        image_list = doc.get_page_images(page.number, full=True)
        if image_list:
            logo = image_list[0]
            rect = page.getImageBbox(logo)
            rect = list(rect)
            rect[0] = rect[0] + 60
            rect[1] = rect[1] - 10
            img = open("图片1.png", "rb").read()
            page.insertImage(rect, stream=img)

doc._deleteObject(logo[0])
# 另存为
doc.save('111.pdf')
doc.close()

```

