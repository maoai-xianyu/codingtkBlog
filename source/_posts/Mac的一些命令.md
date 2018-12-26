---
title: Mac的一些命令
date: 2018-12-26 15:33:00
tags:
	- tools
	- mac
categories: "tools"
---

## mac命令

总结一些常用的命令行，用于熟练和记忆


1. 查看文件MD5：

```
md5[空格][拖曳要检测的文件到此处]

命令 md5 -s 123455667

MD5 ("123455667") = aa8145f5aba38263cebc4038d0ebe849

```
<!--more-->

2. 查看文件SHA1:

```
openssl dgst -sha1[空格][拖曳要检测的文件到此处]

```

3. 查看文件SHA256

```
openssl dgst -sha256[空格][拖曳要检测的文件到此处]

命令 openssl dgst -sha256 /Users/zhangkun/Downloads/gradle-4.6-all.zip

SHA256(/Users/zhangkun/Downloads/gradle-4.6-all.zip)= 9af7345c199f1731c187c96d3fe3d31f5405192a42046bafa71d846c3d9adacb

```