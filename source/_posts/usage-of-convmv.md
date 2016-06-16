---
title: 如何在apache2上部署文件名为中文的网页
date: 2016-06-12 01:22:04
toc: true
tags: 
    - wiki   
    - linux
    - apache
---

## 安装软件
1.  安装中文语言包：`sudo apt install language-pack-zh-hans language-pack-zh-hans-base`。
2.  convmv： Linux下的文件名转码工具，使用`sudo apt install convmv`命令进行安装。

## 使用方法
在Ubuntu中解压中文的html文件名的zip包时，在安装中文语言包的情况下，有时能自动转换成正确中文编码字符，但是有时解压出来的文件名是乱码，可以按照下面的指令解决乱码问题。
```shell
# 加上-O表示为指定系统的编码方式，可以使用locale命令查看系统编码方式。
unzip -O UTF-8 xxx.zip
# 将windows下默认的GBK编码文件名转换成Linux下的UTF-8编码文件名。
sudo convmv -f GBK -t UTF-8 --notest -r xxx/*
```
上面指令中的**xxx** 是你自己的zip文件名和解压以后的文件夹名称。