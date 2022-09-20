---
title: Centos7系统安装LAMP开发环境
author: liuliunx
coverImg: /medias/banner/4.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
  LAMP是指一组通常一起使用来运行动态网站或者服务器的自由软件名称首字母缩写。L指Linux，A指Apache，M一般指MySQL，也可以指MariaDB，P一般指PHP，也可以指Perl或Python。
tags:
  - Centos7
  - 服务器系统环境搭建教程 
  - LAMP
categories:
  - 运维篇
abbrlink: d74d8b44
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---

## 一. 安装CentOS7

## 二、安装Apache

### 1. 安装
```bash
yum -y install httpd
```
### 2. 开启`apache`服务
```bash
systemctl start httpd.service
```
### 3. 设置`apache`服务开机启动
```bash
systemctl enable httpd.service
```
### 4. 验证`apache`服务是否安装成功

在本机浏览器中输入虚拟机的`ip`地址，`CentOS7`查看`ip`地址的方式为：
```bash
ip addr
```
（阿里云不需要用这种方式查看，外网`ip`已经在你主机列表那里给你写出来了的；）

这里是访问不成功的

（阿里云用外网访问，能成功，不需要做以下步骤）

查了资料，说法是，`CentOS7`用的是`Firewall-cmd`，`CentOS7`之前用的是`iptables`防火墙；要想让外网能访问到`apache`主目录，就需要做以下的操作：
```bash
firewall-cmd \--permanent \--zone=public \--add-service=http

firewall-cmd \--permanent \--zone=public \--add-service=https

firewall-cmd \--reload
```
然后再访问外网`ip`，如果看到apache默认的页面`\--有Testing123\...`字样，便是成功安装了`apache`服务了；

## 三、 安装PHP

### 1.安装
```bash
yum -y install php
```
### 2. 重启apache服务
```bash
systemctl restart httpd或者systemctl restart httpd.service
```
然后，你可以写一个php文件在浏览器中运行一下了;

**eg:**

**vi /var/www/html/info.php**

**i**

**\<?php phpinfo(); ?\>**

**Esc**

**:wq**

然后，在自己电脑浏览器输入 192.168.1.1/info.php

运行，会出现php的一些信息

四、安装MySQL

我这里根据所学的那个教程，也安装了MariaDB

1.安装

**yum -y install mariadb mariadb-server**

2.开启MySQL服务

**systemctl start mariadb.service**

3.设置开机启动MySQL服务

**systemctl enable mariadb.service**

4.设置root帐户的密码

**mysql_secure_installation**

然后会出现一串东西，可以仔细读一下，如果你懒得读，就在提示出来的时候，按Enter就好了，让你设置密码的时候，你就输入你想要的密码就行，然后继续在让你选择y/n是，Enter就好了；当一切结束的时候，你可以输入**mysql
-uroot -p**的方式，验证一下；

五、将PHP和MySQL关联起来

**yum search php**，选择你需要的安装：**yum -y install php-mysql**

六、安装常用的PHP模块

例如，GD库，curl，mbstring,\...

1.安装：

**yum -y install php-gd php-ldap php-odbc php-pear php-xml php-xmlrpc
php-mbstring php-snmp php-soap curl curl-devel**

2.重启apache服务

**systemctl restart httpd.service**

然后，再次在浏览器中运行info.php，你会看到安装的模块的信息；

至此，LAMP环境就搭建好了。
