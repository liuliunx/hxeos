---
title: XenServer安装流程
author: liuliunx
coverImg: /medias/banner/13.jpg
top: true
cover: true
toc: true
mathjax: false
summary: >-
  Pandoc是由John MacFarlane开发的标记语言转换工具，可实现不同标记语言间的格式转换.
tags:
  - XenServer
  - 服务器系统环境搭建教程 
categories:
  - 运维篇
abbrlink: d74d8b40
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---
### 1. 下载XenServer

#### 1.XenServer下载安装包链接

百度网盘链接: https://pan.baidu.com/s/1a6C8q3Ey1puWGJ8iQ6wigw 提取码: `mvxg` 

### 2. 安装XenServer
点击安装包回车，直接安装，出现下面界面：
(图片在上文字在下)
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image1.jpeg)

#### 1. 键盘模式：默认US OK

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image2.jpeg)

`OK`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image3.jpeg)

#### 2. 版权声明界面，Accept EULA

回车

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image4.jpeg)

#### 3. 上图表示服务器CPU没有开启虚拟化，请检查BIOS设置，开启虚拟化后继续操作。

**注意：不开启虚拟化，不能安装64位操作系统**

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image5.jpeg)

#### 4. 选择第一项，安装在本地磁盘

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image6.jpeg)

#### 5. 选择安装介质，选择第一项，本地光盘

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image7.jpeg)
#### 6. 选择No，不安装补丁包

![g](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image8.jpeg)

#### 7. 选择第一项，跳过安装介质检测

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image9.jpeg)

#### 8. 设置root密码

`OK`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image10.jpeg)

#### 9. 网络设置界面

选择第二项，设置静态IP、子网掩码、网关

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image11.jpeg)

#### 10. Hostname:设置主机名，自定义设置DNS地址：根据自己的需要设置

`OK`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image12.jpeg)
#### 11. 选择区域：Asia

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image13.jpeg)

#### 12. 选择时区：Shanghai

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image14.jpeg)

#### 13. 服务器时间设置

第一项:使用`NTP`服务器来同步时间

第二项：使用本地时间

这里选择本地时间

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image15.jpeg)

#### 14. 开始安装

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image16.jpeg)

#### 15. 正在安装中

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image17.jpeg)

#### 16. 设置时间

`Ok`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image18.jpeg)

#### 17. 安装完成

点`Ok`，系统重启

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image19.jpeg)

#### 18. 系统启动成功，进入下面界面：

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/XenServer/image20.jpeg)

在此界面可以对服务器网络、主机名等进行设置。
