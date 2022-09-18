---
title: word文档转markdown格式【pandoc的使用方法】
author: liuliunx
coverImg: /medias/banner/13.jpg
top: true
cover: true
toc: true
mathjax: false
summary: >-
  Pandoc是由John MacFarlane开发的标记语言转换工具，可实现不同标记语言间的格式转换.
tags:
  - markdown
  - word 
  - pandoc
  - windows
categories:
  - 博客篇
abbrlink: d74d8b39
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---
### 0x01 Pandoc的安装

`word`文档转`md`格式有多种方法，这里推荐的是`Pandoc`，从`Pandoc`官网下载 
示例如下：

#### 1. 点击`Installing`
进入下载页面，页面会自动选择跟电脑系统合适安装版本，也可以从右边选择要下载系统安装版本



#### 2. 直接点击 `Download the latest installer for Windows (64-bit)`（安装版本）
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/pandoc/image3.png)


#### 3. 下载好安装包，安装时候需要退出360等管家，不然会报拦截信息，运行安装包后点同意后面安装很快
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/pandoc/image4.png)


#### 4. `pandoc.exe`程序默认安装路径在`C:\\Users\\Administrator\\AppData\\Local\\Pandoc`
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/pandoc/image5.png)

 

### 0x02 Pandoc的使用

#### 1. 在路径搜索框输入`cmd `再按回车会自动出现命令行

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/pandoc/image1.png)

#### 2. 以下的语法格式为将一篇`word`文档转换为`markdown`格式：

有图片的话生成的图片在`\\images\\media`这个文件夹目录下。

`aaa.md`是生产md文件的文件名

`aaa.docx`需要转换原文件名

#### 3. 需要执行的以下命令就是：

```bash
pandoc -f docx -t markdown \--extract-media ./images -o aaa.md aaa.docx
```

#### 4. 需要注意的是转的文档需要和`pandoc.exe`在同一层级，否则路径错误执行是不会成功的

示例如下：

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/pandoc/image2.png)
