---
title: Liunx上的Psdash监控软件的安装
author: liuliunx
coverImg: /medias/banner/13.jpg
top: true
cover: true
toc: true
mathjax: false
summary: >-
  psdash-为开发、测试人员提供简单的方法，在web界面查看服务器的运行情况（网络，带宽，磁盘，CPU）， 同时可以在web界面查看日志.
tags:
  - psdash
  - Liunx 
categories:
  - 运维篇
abbrlink: d74d8b54
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---


## 一. 安装脚本名：`psdash.sh`

### 1. 安装主控服务器脚本：
```bash
1 yum -y groupinstall "Development Tools"
 2 yum -y install python-devel
 3 wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
 4 wget https://pypi.python.org/packages/source/s/setuptools/setuptools-18.3.2.tar.gz#md5=d30c969065bd384266e411c446a86623 --no-check-certificate
 5 tar -zxvf setuptools-18.3.2.tar.gz
 6 cd setuptools-18.3.2
 7 python setup.py install
 8 cd ..
 9 wget "https://pypi.python.org/packages/source/p/pip/pip-1.5.4.tar.gz#md5=834b2904f92d46aaa333267fb1c922bb" --no-check-certificate
10 tar -zxvf pip-1.5.4.tar.gz
11 cd pip-1.5.4
12 python setup.py install
13 pip install psdash
14 psdash &
```
### 2. 客户端的安装脚本：
```bash
1 yum -y groupinstall "Development Tools"
 2 yum -y install python-devel
 3 wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
 4 wget https://pypi.python.org/packages/source/s/setuptools/setuptools-18.3.2.tar.gz#md5=d30c969065bd384266e411c446a86623 --no-check-certificate
 5 tar -zxvf setuptools-18.3.2.tar.gz
 6 cd setuptools-18.3.2
 7 python setup.py install
 8 cd ..
 9 wget "https://pypi.python.org/packages/source/p/pip/pip-1.5.4.tar.gz#md5=834b2904f92d46aaa333267fb1c922bb" --no-check-certificate
10 tar -zxvf pip-1.5.4.tar.gz
11 cd pip-1.5.4
12 python setup.py install
13 pip install psdash
14 psdash -a --register-to http://xxx.xxx.xxx.xxx:5000 --register-as $1 &
```
## 二. `bash psdash.sh `注册主机名 &

### 1. 遇到问题：

客户端连接不上主控服务器，原因是主控服务器防火墙没有关闭：

#### 1. 查看防火墙状态
```bash
firewall-cmd --state
```
#### 2. 停止`firewall`
```bash
systemctl stop firewalld.service
```
#### 3. 禁止`firewall`开机启动
```bash
systemctl disable firewalld.service
```
#### 4. 查看本地`IP`地址：
```bash
ifconfig
```
### 2. 发现` ens33 `没有 ` inet `这个属性，那么就没法通过`IP`地址连接虚拟机。

#### 1. 接着来查看`ens33`网卡的配置： 
```bash
vi /etc/sysconfig/network-scripts/ifcfg-ens33   
```
 注意`vi`后面加空格

#### 2. 从配置清单中可以发现 `CentOS 7` 默认是不启动网卡的`ONBOOT=no`。
```bash
 把这一项改为`YES` ` ONBOOT=yes`，
```
#### 3. 然后按` Esc `退出  再出入命令 :` wq ` 再按`Enter`即可  

#### 4. 然后重启网络服务： 
```bash
sudo service network restart
```