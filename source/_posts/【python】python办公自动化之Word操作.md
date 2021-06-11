---
title: 【python】python办公自动化之Word操作
tags:
  - python
abbrlink: d86cf53c
date: 2021-05-26 09:32:10
---



## 前言

内容已学习完，文章待整理。

教程来自bilibili

https://blog.cdn.ionluo.cn/pdf/python-docx%E7%AC%94%E8%AE%B0.pdf



## 具体操作

### 提取Word图片

```python
# coding:utf-8

import os
import shutil
import zipfile
import docx
import os, re


# 利用三方库docx实现图片提取(推荐)
def get_pictures_by_docx(word_path, result_path):
    """
    图片提取
    :param word_path: word文件路径
    :param result_path: 图片保存目录路径
    :return:
    """
    print(f'word_path: {word_path}\n')

    if not os.path.exists(result_path):
        os.makedirs(result_path)

    doc = docx.Document(word_path)
    # https://python-docx.readthedocs.io/en/latest/api/document.html?highlight=part#docx.document.Document.part
    dict_rel = doc.part._rels
    for rel in dict_rel:
        rel = dict_rel[rel]
        if "image" in rel.target_ref:
            #  media/image3.jpeg
            img_dir = os.path.basename(word_path).split('.')[0]
            img_name = rel.target_ref.split('/')[-1]
            img_abs_dir = os.path.join(result_path, img_dir)
            img_abs_name = os.path.join(img_abs_dir, img_name)
            if not os.path.exists(img_abs_dir):
                os.makedirs(img_abs_dir)
            with open(img_abs_name, "wb") as f:
                f.write(rel.target_part.blob)

# # 遍历文件夹
# def walkFile(file):
#     for root, dirs, files in os.walk(file):
#         # root 表示当前正在访问的文件夹路径
#         # dirs 表示该文件夹下的子目录名list
#         # files 表示该文件夹下的文件list
#
#         # 遍历文件
#         for f in files:
#             print(os.path.join(root, f))
#         # 遍历所有的文件夹
#         for d in dirs:
#             print(os.path.join(root, d))


# 获取一个目录下的所有word文件
def get_all_word(cwd, result=[]):
    files = os.listdir(cwd)
    for file in files:
        sub_dir = os.path.join(cwd, file)
        if os.path.isdir(sub_dir):
            get_all_word(sub_dir, result)
        elif os.path.splitext(sub_dir)[-1] in ['.docx', '.doc']:
            result.append(sub_dir)
    return result


# 利用word文件是一个zip压缩包的原理提取图片
def get_pictures_by_zip(word_path, result_path):
    """
    图片提取
    :param word_path: word文件路径
    :param result_path: 图片保存目录路径
    :return:
    """
    print(f'word_path: {word_path}\n')

    if not os.path.exists(result_path):
        os.makedirs(result_path)

    # 解压后文件存放路径
    tmp_path = f'{os.path.splitext(word_path)[0]}'
    # word文件的拷贝文件（为了以防万一，先拷贝源文件再解压）
    zip_path = shutil.copy(word_path, f'{os.path.splitext(word_path)[0]}_new{os.path.splitext(word_path)[1]}')
    # 解压word文件
    with zipfile.ZipFile(zip_path, 'r') as f:
        for file in f.namelist():
            print("file", file)
            f.extract(file, tmp_path)
    # 删除word文件的拷贝文件
    os.remove(zip_path)
    # word图片在解压文件内的word/media目录下
    pic_path = os.path.join(tmp_path, 'word/media')
    # 如果没有图片文件，结束，返回提示
    if not os.path.exists(pic_path):
        shutil.rmtree(tmp_path)
        return 'no pictures found'
    # 读取目录下的所有文件并拷贝到结果文件夹内（根据word文件名创建文件夹存储这些图片）
    pictures = os.listdir(pic_path)
    for picture in pictures:
        img_dir = os.path.basename(word_path).split('.')[0]
        img_name = picture
        img_abs_dir = os.path.join(result_path, img_dir)
        img_abs_name = os.path.join(img_abs_dir, img_name)
        if not os.path.exists(img_abs_dir):
            os.makedirs(img_abs_dir)
        shutil.copy(os.path.join(pic_path, picture), img_abs_name)
    shutil.rmtree(tmp_path)


if __name__ == '__main__':
    # current_path = os.getcwd()
    # result_path = os.path.join(word_path, '..', 'wordImage')

    word_path = r'E:\project\EDM\demo\python\word\newFile'
    result_path = os.path.join(word_path, 'wordImage')

    word_paths = get_all_word(word_path, [])
    print(f'\n当前路径下有word文件{len(word_paths)}个\n')
    for path in word_paths:
        # get_pictures_by_docx(path, result_path)
        get_pictures_by_zip(path, result_path)

    print('word图片提取完成，图片位置：' + result_path)
```

### 替换word图片

如果对word有足够的了解，理论上可以做到任何word编辑操作。

使用该方式，还可以实现word模板功能，和python库python-docx-template一样的功能。

```python
# coding:utf-8

import os
import shutil
import zipfile
import docx
import os, re


# 遍历获取文件夹下的所有文件，用于写入压缩文件(返回的结果是相对于root的相对路径)
# 注意这里的写法，result如果要初始化为空不能不传，因为是一个可变对象，传的是指针
def dfs_get_zip_file(cwd, result=[], root=None):
    root = root or cwd
    files = os.listdir(cwd)
    for file in files:
        sub_dir = os.path.join(cwd, file)
        if os.path.isdir(sub_dir):
            dfs_get_zip_file(sub_dir, result, root)
        else:
            result.append(sub_dir.split(root+os.sep)[-1])
    return result


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


# 1. 复制所有的word文件到一个新的位置
# 2. 遍历里面的word文件，解压到文件夹中并删除之
# 3. 遍历解压后的word文件夹，找出封面图片并替换
# 4. 遍历解压后的word文件夹，将其还原回word文件并删除之
def replaceImage(word_path, image_path):
    """
    替换word的封面图片
    :param word_path: word文件夹路径
    :param image_path: 替换用的图片路径
    :return:
    """
    print(f'\nWord文件位置: {word_path}\n')

    # 1. 复制所有的word文件到一个新的位置
    # PermissionError: [Errno 13] Permission denied:
    # https://stackoverflow.com/questions/11278066/using-shutil-copyfile-i-get-a-python-ioerror-errno-13-permission-denied
    # https://www.jb51.net/article/87984.htm
    copy_path = os.path.abspath(os.path.join(word_path, '..', 'copyFile'))
    print('复制文件到：' + copy_path)
    if os.path.exists(copy_path):
        # os.makedirs(copy_path)
        _continue = input("Target position is not null! We will replace it, continue? (yes or no)\n> ")
        if _continue in ['yes', 'y', 'Y']:
            shutil.rmtree(copy_path)
        else:
            return 'interrupt'
    shutil.copytree(word_path, copy_path)

    print('\n')

    # 2. 遍历里面的word文件，解压到文件夹中并删除之
    word_files = dfs_get_word_file(copy_path, [])
    for word in word_files:
        print('解压word文件：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径
        word_unzip_folder = word_abs_path.split('.')[0]  # 解压word存放文件夹路径
        # 解压
        with zipfile.ZipFile(word_abs_path, 'r') as f:
            for file in f.namelist():
                f.extract(file, word_unzip_folder)
        # 删除原来的word文件
        os.remove(word_abs_path)

    print('\n')

    # 3. 遍历解压后的word文件夹，找出封面图片并替换（这里通过文件大小来判断是否是封面图的）
    for word in word_files:
        print('替换word文件封面图片：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径
        word_unzip_folder = word_abs_path.split('.')[0]  # 解压word存放文件夹路径
        # 图片位置在word/media
        image_dir = os.path.join(word_unzip_folder, 'word', 'media')
        if os.path.exists(image_dir):
            image_list = os.listdir(image_dir)
            for image in image_list:
                cover = os.path.join(image_dir, image)
                if os.path.getsize(cover) == 591035:
                    os.remove(cover)
                    shutil.copyfile(image_path, cover)

    print('\n')

    # 4. 遍历解压后的word文件夹，将其还原回word文件并删除之
    for word in word_files:
        print('将word文件夹压缩回word文件：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径
        word_unzip_folder = word_abs_path.split('.')[0]  # 解压word存放文件夹路径

        zipFile = zipfile.ZipFile(word_abs_path, 'w')
        results = dfs_get_zip_file(word_unzip_folder, [])
        for result in results:
            zipFile.write(os.path.join(word_unzip_folder, result), result)
        zipFile.close()
        # 删除解压后的文件夹
        shutil.rmtree(word_unzip_folder)


if __name__ == '__main__':
    # word_path = r'E:\project\EDM\demo\python\word\newFile'
    image_path = r"E:\project\EDM\demo\python\word\file\图片1.png"

    replaceImage(word_path, image_path)
```



### 复制word文档内容到新的文件

该方式有一个很麻烦的点，就是速度太慢了，处理一个文档就要大概2-5秒。

关于win32com的使用，暂时没有找到好的文档，下面是一些我的参考文章。

- https://blog.csdn.net/swkfk/article/details/113856618
- https://blog.csdn.net/weixin_39588223/article/details/110780845
- https://blog.csdn.net/qdPython/article/details/114439716
- https://www.cnblogs.com/zhuminghui/p/11765401.html

```python
# coding:utf-8

# 文档 https://docs.microsoft.com/zh-cn/office/vba/api/word.application.activedocument
import win32com, os, sys, re
from win32com.client import Dispatch, constants
from docx import Document

# def OpenNewFile():
#     word = Dispatch('Word.Application')
#     # 或者使用下面的方法，使用启动独立的进程：
#     # word = DispatchEx('Word.Application')
#
#     # 如果不声明以下属性，运行的时候会显示的打开word
#     word.Visible = 0  # 0:后台运行 1:前台运行(可见)
#     word.DisplayAlerts = 0  # 不显示，不警告
#
#     # 创建新的word文档
#     doc = word.Documents.Add()
#
#     # 在文档开头添加内容
#     myRange1 = doc.Range(0, 0)
#     myRange1.InsertBefore('Hello word\n')
#
#     # 在文档末尾添加内容
#     myRange2 = doc.Range()
#     myRange2.InsertAfter('Bye word\n')
#
#     # 在文档i指定位置添加内容
#     i = 0
#     myRange3 = doc.Range(0, i)
#     myRange3.InsertAfter("what's up, bro?\n")
#
#     # doc.Save()  # 保存
#     doc.SaveAs(os.getcwd() + "\\funOpenNewFile.docx")  # 另存为
#     doc.Close()  # 关闭 word 文档
#     word.Quit()  # 关闭 office

def copy2Template(word_path, template_path, result_path):
    """
    复制word文件内容到模板word中，实现模板替换（这里就和手动的复制粘贴一样）
    :param word_path: word文件路径
    :param template_path: word模板文件路径
    :param result_path: 替换后保存文件夹路径
    :return:
    """
    print('\n')
    print(f'Word文件位置: {word_path}\n')
    print(f'Word模板位置: {template_path}\n')
    print(f'Word保存位置: {result_path}\n')

    if not os.path.exists(result_path):
        os.makedirs(result_path)

    w = win32com.client.Dispatch('Word.Application')
    # 或者使用下面的方法，使用启动独立的进程（推荐）：
    # w = win32com.client.DispatchEx('Word.Application')

    w.Visible = 0  # 0:后台运行 1:前台运行(可见)
    w.DisplayAlerts = 0  # 不显示，不警告
    
    # 导致下面错误的原因可能是：1. 绝对路径错误  2. 路径包含中文
    # pywintypes.com_error: (-2147023170, '远程过程调用失败。', None, None)
    # pywintypes.com_error: (-2147023174, 'RPC 服务器不可用。', None, None)
    word = w.Documents.Open(word_path)
    template = w.Documents.Open(template_path)

    # print(file.paragraphs[0].Copy())
    word.Activate()
    w.ActiveDocument.Sections[0].Range.Copy()
    template.Activate()
    w.ActiveDocument.Sections[0].Range.Paste()

    word_new_path = os.path.join(result_path, os.path.basename(word_path))
    w.ActiveDocument.SaveAs(word_new_path)

    # template.Activate()
    # w.ActiveDocument.Sections[0].Headers[0].Range.Copy()
    # file.Activate()
    # w.ActiveDocument.Sections[0].Headers[0].Range.Paste()
    # template.Activate()
    # w.ActiveDocument.Sections[0].Footers[0].Range.Copy()
    # file.Activate()
    # w.ActiveDocument.Sections[0].Footers[0].Range.Paste()
    # myRange = w.ActiveDocument.Range(10)
    # myRange.InsertBefore('Hello from Python!')

    word.Close()
    template.Close()

    try:
        w.Documents.Close()
        w.Quit()
        print('end')
    except Exception as e:
        print('error', e)

if __name__ == '__main__':
    word_path = os.path.join(os.getcwd(), 'file', 'file.docx')
    template_path = os.path.join(os.getcwd(), 'file', 'template.docx')
    result_path = os.path.join(os.getcwd(), 'newFile')
    copy2Template(word_path, template_path, result_path)
```



### 汇总上面实现的word文件模板替换需求

```python
# coding:utf-8

import os
import shutil
import zipfile
import docx
import os, re
import win32com
from win32com.client import Dispatch, constants
from docx import Document


# 遍历获取文件夹下的所有文件，用于写入压缩文件(返回的结果是相对于root的相对路径)
# 注意这里的写法，result如果要初始化为空不能不传，因为是一个可变对象，传的是指针
def dfs_get_zip_file(cwd, result=[], root=None):
    root = root or cwd
    files = os.listdir(cwd)
    for file in files:
        sub_dir = os.path.join(cwd, file)
        if os.path.isdir(sub_dir):
            dfs_get_zip_file(sub_dir, result, root)
        else:
            result.append(sub_dir.split(root+os.sep)[-1])
    return result


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


# 删除word中的某个段落
def delete_paragraph(paragraph):
    """
    删除word中的某个段落
    :param paragraph: 需要删除的段落
    :return:
    """
    p = paragraph._element
    p.getparent().remove(p)
    paragraph._p = paragraph._element = None


# 1. 复制所有的word文件到一个新的位置
# 2. 遍历里面的word文件，解压到文件夹中并删除之
# 3. 遍历解压后的word文件夹，找出封面图片并替换
# 4. 遍历解压后的word文件夹，将其还原回word文件并删除之
# 5. 将还原后的word文件内容复制到模板文件中，并替换之
# 6. 将word文件的最后的空白页删除
def replaceTemplate(word_path, template_path, cover_image, header_image, footer_image):
    """
    替换word的封面图片
    :param word_path: word文件夹路径
    :param cover_image: 替换用的封面图片路径
    :param header_image: 替换用的页头图片路径
    :param footer_image: 替换用的页脚图片路径
    :return:
    """
    print(f'\nWord文件位置: {word_path}\n')

    # 1. 复制所有的word文件到一个新的位置
    # PermissionError: [Errno 13] Permission denied:
    # https://stackoverflow.com/questions/11278066/using-shutil-copyfile-i-get-a-python-ioerror-errno-13-permission-denied
    # https://www.jb51.net/article/87984.htm
    copy_path = os.path.abspath(os.path.join(word_path, '..', 'copyFile'))
    print('复制文件到：' + copy_path)
    if os.path.exists(copy_path):
        # os.makedirs(copy_path)
        _continue = input("Target position is not null! We will replace it, continue? (yes or no)\n> ")
        if _continue in ['yes', 'y', 'Y']:
            shutil.rmtree(copy_path)
        else:
            return 'interrupt'
    shutil.copytree(word_path, copy_path)

    print('\n')

    # 2. 遍历里面的word文件，解压到文件夹中并删除之
    word_files = dfs_get_word_file(copy_path, [])
    for word in word_files:
        print('解压word文件：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径
        word_unzip_folder = word_abs_path.split('.')[0]  # 解压word存放文件夹路径
        # 解压
        with zipfile.ZipFile(word_abs_path, 'r') as f:
            for file in f.namelist():
                f.extract(file, word_unzip_folder)
        # 删除原来的word文件
        os.remove(word_abs_path)

    print('\n')

    # 3. 遍历解压后的word文件夹，找出封面图片并替换（这里通过文件大小来判断是否是封面图的）
    for word in word_files:
        print('替换word文件封面图片：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径
        word_unzip_folder = word_abs_path.split('.')[0]  # 解压word存放文件夹路径
        # 图片位置在word/media
        image_dir = os.path.join(word_unzip_folder, 'word', 'media')
        if os.path.exists(image_dir):
            image_list = os.listdir(image_dir)
            for image in image_list:
                img = os.path.join(image_dir, image)
                if cover_image and os.path.getsize(img) == 591035:
                    os.remove(img)
                    shutil.copyfile(cover_image, img)
                if header_image and os.path.getsize(img) == 51078:
                    os.remove(img)
                    shutil.copyfile(header_image, img)
                if footer_image and os.path.getsize(img) == 71086:
                    os.remove(img)
                    shutil.copyfile(footer_image, img)

    print('\n')

    # 4. 遍历解压后的word文件夹，将其还原回word文件并删除之
    for word in word_files:
        print('将word文件夹压缩回word文件：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径
        word_unzip_folder = word_abs_path.split('.')[0]  # 解压word存放文件夹路径

        zipFile = zipfile.ZipFile(word_abs_path, 'w')
        results = dfs_get_zip_file(word_unzip_folder, [])
        for result in results:
            zipFile.write(os.path.join(word_unzip_folder, result), result)
        zipFile.close()
        # 删除解压后的文件夹
        shutil.rmtree(word_unzip_folder)

    print('\n')

    # 5. 将还原后的word文件内容复制到模板文件中，并替换之
    for word in word_files:
        print('将还原后的word文件内容复制到模板文件中，并替换之：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径

        # w = win32com.client.Dispatch('Word.Application')
        # 或者使用下面的方法，使用启动独立的进程：
        w = win32com.client.DispatchEx('Word.Application')

        w.Visible = 0  # 0:后台运行 1:前台运行(可见)
        w.DisplayAlerts = 0  # 不显示，不警告

        word = w.Documents.Open(word_abs_path)
        template = w.Documents.Open(template_path)

        word.Activate()
        w.ActiveDocument.Sections[0].Range.Copy()
        template.Activate()
        w.ActiveDocument.Sections[0].Range.Paste()
        # 导致下面错误的原因可能是：1. 绝对路径错误  2. 路径包含中文
        # pywintypes.com_error: (-2147023170, '远程过程调用失败。', None, None)
        # pywintypes.com_error: (-2147023174, 'RPC 服务器不可用。', None, None)
        word.Close()
        w.ActiveDocument.SaveAs(word_abs_path)
        template.Close()

        try:
            w.Documents.Close()
            w.Quit()
        except Exception as e:
            print('error', e)

    print('\n')

    # 6. 将word文件的最后的空白页删除
    for word in word_files:
        print('将word文件的最后的空白页删除：' + word)
        word_abs_path = os.path.join(copy_path, word)  # 该word文件的绝对路径

        file = Document(word_abs_path)
        length = len(file.paragraphs)
        # 删除最后的空白内容
        while length > 0 and not file.paragraphs[length - 1].text:
            delete_paragraph(file.paragraphs[-1])
            length = length - 1
        file.save(word_abs_path)

    print('end\n')


if __name__ == '__main__':
    # word_path = r'E:\project\EDM\demo\python\word\newFile'
    template_path = r'E:\project\EDM\demo\python\word\file\template.docx'
    cover_image = r"E:\project\EDM\demo\python\word\file\图片1.png"
    # header_image = r"E:\project\EDM\demo\python\word\file\图片2.png"
    # footer_image = r"E:\project\EDM\demo\python\word\file\图片3.png"
    header_image = footer_image = None
    replaceTemplate(word_path, template_path, cover_image, header_image, footer_image)
```



### WORD转PDF（Window系统）

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
    word_path = r'E:\虚拟机共享文件夹\官网\Oricell\产品手册'
    pdf_path = r'E:\虚拟机共享文件夹\官网\Oricell\产品手册PDF'

    word_files = dfs_get_word_file(word_path, [])
    for file in word_files:
        doc_name = os.path.join(word_path, file)
        pdf_name = os.path.join(pdf_path, file.split('.')[0] + '.pdf')
        doc2pdf(doc_name, pdf_name)
    print('done.')

```







## 参考文献

[用Python拷贝word图片到指定文件](https://mp.weixin.qq.com/s/DgkuSxZrx5v2SFoqows9Kg)

[Python自动化办公之Word，全网最全看这一篇就够了](https://mp.weixin.qq.com/s/gWEbrEAuPN1pEJPzJOVD3g)

[python如何提取word内的图片](https://www.cnblogs.com/rongge95500/p/12666308.html)

[最全总结 | 聊聊 Python 办公自动化之 Word](https://blog.csdn.net/hsh881025/article/details/109712799)

[python-docx](https://python-docx.readthedocs.io/en/latest/)

[Python标准库shutil用法实例详解](https://www.jb51.net/article/145522.htm)

[python中zipfile模块实例化解析](https://www.cnblogs.com/gufengchen/p/10956009.html)