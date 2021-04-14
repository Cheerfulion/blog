---
title: Ubuntu安装WPS
tags:
  - 运维
  - Linux
abbrlink: db88cc42
date: 2021-04-09 13:46:32
---



## 下载

官网下载地址：http://www.wps.cn/product/wpslinux



选择**64位Deb格式**安装包：

![image-20210409134826503](http://blog.cdn.ionluo.cn/blog/image-20210409134826503.png)





## 安装

```bash
# 切换到安装包目录(中文环境是下载)
cd Download
sudo dpkg -i wps-office_11.1.0.10161_amd64.deb
```

![image-20210409134954313](http://blog.cdn.ionluo.cn/blog/image-20210409134954313.png)

注意，此时直接搜索打开会提示缺少字体。



## 字体安装

测试系统：Ubuntu16.04

1. **下载字体** ：[wps_symbol_fonts.zip](https://blog.cdn.ionluo.cn/files/wps_symbol_fonts.zip)

2. **解压安装字体**

   ```bash
   sudo unzip -d ./wps_symbol_fonts/ wps_symbol_fonts.zip
   sudo cp wps_symbol_fonts/* /usr/share/fonts/wps_fonts
   ```

3. **大功告成**



## 打开WPS

1.  **Ubuntu点击搜索打开**

   ![image-20210409135608627](http://blog.cdn.ionluo.cn/blog/image-20210409135608627.png)

2. **命令行打开**

   ```bash
   "et"          -->      "WPS表格程序"，
   "wps"         -->      "WPS文字程序"，
   "wpp"         -->      "WPS演示程序"
   
   
   # 后面加上文件名即是编辑该文件
   wps a.doc
   
   # -w ： 新窗口打开
   wps -w a.doc
   ```

   

