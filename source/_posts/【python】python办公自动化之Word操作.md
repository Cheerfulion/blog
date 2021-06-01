---
title: 【python】python办公自动化之Word操作
tags:
  - python
abbrlink: d86cf53c
date: 2021-05-26 09:32:10
---



## 前言

内容已学习完，文章待整理。



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

如果对word有足够的了解，理论上可以做到任何word编辑操作

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





## 参考文献

[用Python拷贝word图片到指定文件](https://mp.weixin.qq.com/s/DgkuSxZrx5v2SFoqows9Kg)

[Python自动化办公之Word，全网最全看这一篇就够了](https://mp.weixin.qq.com/s/gWEbrEAuPN1pEJPzJOVD3g)

[python如何提取word内的图片](https://www.cnblogs.com/rongge95500/p/12666308.html)

[最全总结 | 聊聊 Python 办公自动化之 Word](https://blog.csdn.net/hsh881025/article/details/109712799)

[python-docx](https://python-docx.readthedocs.io/en/latest/)