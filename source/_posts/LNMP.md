---
title: Centos系统安装LNMP
author: liuliunx
coverImg: /medias/banner/9.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
  LNMP是指一组通常一起使用来运行动态网站或者服务器的自由软件名称首字母缩写。L指Linux，N指Nginx，M一般指MySQL，也可以指MariaDB，P一般指PHP，也可以指Perl或Python。
tags:
  - Centos
  - 服务器系统环境搭建教程 
  - LNMP
categories:
  - 运维篇
abbrlink: d74d8b53
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---
### 1. 用Xshell5连接Centos系统

登陆后运行：
```bash
screen -S lnmp
```
如果提示`screen: command not found `命令不存在可以执行：
```bash
yum install screen或 apt-get install screen安装
```
然后安装`LNMP`稳定版，可用无人值守一键安装包 ：
```bash
wget http://soft.vpser.net/lnmp/lnmp1.5.tar.gz -cO lnmp1.5.tar.gz && tar zxf lnmp1.5.tar.gz && cd lnmp1.5 && LNMP_Auto="y" DBSelect="1" DB_Root_Password="hoolai@123" InstallInnodb="y" PHPSelect="2" SelectMalloc="1" ./install.sh lnmp
```
### 2. 如需要安装LNMPA或LAMP
将`./install.sh `后面的参数`lnmp`替换为`lnmpa`或`lamp`即可。如需更改网站和数据库目录、自定义`Nginx`参数、`PHP`参数模块、开启`lua`等需在运行`./install.sh `命令前修改安装包目录下的 `lnmp.conf `文件，详细可以查看`lnmp.conf`文件参数说明。

如提示`wget: command not found `使用
```bash
yum install wget 或 apt-get install wget 命令安装。
```
并且`Nginx、MySQL、PHP`都是`running，80`和`3306`端口都存在，并提示安装使用的时间及`Install lnmp V1.5 completed! enjoy it`的话，说明已经安装成功。

某些系统可能会一直卡在`Install lnmp V1.5 completed! enjoy it`不自动退出，可以按`Ctrl+c`退出。
### 3. 添加虚拟主机
安装完成接下来开始使用就可以了，按添加虚拟主机教程，添加虚拟主机后可以使用`sftp`或`ftp`服务器上传网站代码，将域名解析到`VPS`或服务器的`IP`上，解析生效即可使用。
```bash
\[root@izwz95wmcrzv8ss8ly6p5xz lnmp1.5\]# lnmp vhost add
```
+\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--+
```bash
\| Manager for LNMP, Written by Licess \|
```
+\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--+
```bash
\| https://lnmp.org \|
```
+\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--+
```bash
Please enter domain(example: www.lnmp.org): www.sunbaodi.com

Your domain: www.sunbaodi.com

Enter more domain name(example: lnmp.org \*.lnmp.org):

Please enter the directory for the domain: www.sunbaodi.com

Default directory: /home/wwwroot/www.sunbaodi.com:

Virtual Host Directory: /home/wwwroot/www.sunbaodi.com

Allow Rewrite rule? (y/n) n

You choose rewrite: none

Enable PHP Pathinfo? (y/n) y

Enable pathinfo.

Allow access log? (y/n) y

Enter access log filename(Default:www.sunbaodi.com.log):

You access log filename: www.sunbaodi.com.log

Create database and MySQL user with same name (y/n) y\^Hn\^\[\[D\^\[\[D

Add SSL Certificate (y/n) y

1: Use your own SSL Certificate and Key

2: Use Let\'s Encrypt to create SSL Certificate and Key

Enter 1 or 2: 2
```
### 4. 重启LNMP命令：lnmp restart

### 5. 数据库密码：hoolai@123
