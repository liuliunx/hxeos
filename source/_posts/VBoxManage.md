---
title: 用VBoxManage命令行安装win7虚拟机
author: liuliuxn
coverImg: /medias/banner/15.jpg
top: false
cover: false
toc: true
mathjax: false
summary: '用VBoxManage命令行安装win7虚拟机'
tags:
  - windows7
  - 虚拟机
  - VBoxManage
categories:
  - 运维篇
abbrlink: 4b3510a6
reprintPolicy: cc_by
date: 2020-03-27 00:00:00
img:
password:
---


### 1. 创建虚拟机
```bash
VBoxManage createvm \--name win7-64 \--register
```
### 2. 设置系统类型windows7
```bash
VBoxManage modifyvm win7-64 \--ostype windows7
```
### 3. 设置内存大小2G
```bash
VBoxManage modifyvm win7-64 \--memory 2096
```
### 4. 建立虚拟磁盘：系统盘30G
```bash
VBoxManage createmedium \--filename win7-64_HDD_SYS_100G.vdi \--size
30000
```
### 5. 创建存储控制器IDE、SATA
```bash
VBoxManage storagectl win7-64 \--name IDE \--add ide \--controller PIIX4
\--bootable on
```
```bash
VBoxManage storagectl win7-64 \--name SATA \--add sata \--controller
IntelAhci \--bootable on
```
### 6. 关联虚拟机磁盘
```bash
VBoxManage storageattach win7-64 \--storagectl IDE \--port 0 \--device 0
\--type dvddrive \--medium cn_windows_7\_ultimate_x64_dvd_x15-66043.isoi
```
### 7. 关联镜像文件
```bash
VBoxManage modifyvm win7-64 \--nic1 bridged \--nictype1 82545EM
\--cableconnected1 on \--bridgeadapter1 eth0
```
### 8. 设置网络为桥接（nictype和bridgeadapter要根据主机的实际情况选择）
```bash
VBoxManage modifyvm win7-64 \--vrdeport 5540 \--vrdeaddress \"\"

VBoxManage modifyvm win7-64 \--vrde on

VBoxManage startvm win7-64 \--type headless
```
### 9. 带界面启动：
```bash
vboxmanage startvm win7-64 \--type gui
```