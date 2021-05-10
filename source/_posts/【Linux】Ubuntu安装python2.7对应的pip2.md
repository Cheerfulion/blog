---
title: Ubuntu安装python2.7对应的pip2
description: '-'
tags:
  - Linux
abbrlink: 455e7f2e
date: 2021-05-10 21:01:08
---



1. **先看看系统中是不是有python2 和pip2**

   ```bash
   # 查看系统中所有Python2.7所在的位置
   whereis python2.7
   # 查看系统中所有pip2.7所在的位置
   whereis pip2.7
   # 查看系统默认调用的是哪个位置的Python2.7
   which python2.7
   # 查看系统默认调用的是哪个位置的pip2.7
   which pip2.7
   
   # 一个python版本可以通过python, python2, python2.7三个命令调用，pip同理，其他版本同理。
   ```

   

2. **想要重新安装可以先删除Python2.7**

3. **安装Python2.7**

   https://www.python.org/downloads/

   ```bash
   # 下载包
   wget http://www.python.org/ftp/python/2.7.6/Python-2.7.6.tar.gz
   # 解压包
   tar -zxvf Python-2.7.6.tar.gz
   # 切换到包目录
   cd Python-2.7.6
   # 安装包
   /configure
   make && make install
   # 查看安装是否成功和默认调用的版本
   python -V
   python2 -V
   python2.7 -V
   ```

4. **安装pip（对应Python版本, 这里是对应python2.7）**

   https://pypi.org/project/pip/9.0.1/#history

   ```bash
   # 下载包
   wget http://www.python.org/ftp/python/2.7.6/Python-2.7.6.tar.gz
   # 解压包
   tar -zxvf pip-9.0.1.tar.gz
   # 切换到包目录
   cd pip-9.0.1
   # 安装包
   python2.7 setup.py install
   # 查看安装是否成功和默认调用的版本
   pip -V
   pip2 -V
   pip2.7 -V
   ```

   上面安装包的时候可能会缺少setuptools，如此的话，需要先安装setuptools工具包

   ```bash
   # 下载包
   wget http://pypi.python.org/packages/source/s/setuptools/setuptools-0.6c11.tar.gz
   # 解压包
   tar -xvf setuptools-0.6c11.tar.gz
   # 切换到包目录
   cd setuptools-0.6c11
   # 安装包
   python2.7 setup.py build
   python2.7 setup.py install
   # 安装pip包
   ```

5. **都安装完成后，上面的压缩包和解压的文件就可以删除了。**