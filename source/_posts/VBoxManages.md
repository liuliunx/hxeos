---
title: Ubuntu 16.04 安装VBoxManage
author: liuliuxn
coverImg: /medias/banner/15.jpg
top: false
cover: false
toc: true
mathjax: false
summary: 'Ubuntu 16.04 安装VBoxManage虚拟机环境'
tags:
  - Ubuntu 16.04
  - 虚拟机
  - VBoxManage
categories:
  - 运维篇
abbrlink: 4b3510a7
reprintPolicy: cc_by
date: 2020-03-27 00:00:00
img:
password:
---
### 1. 准备工作

在开始使用`VBoxManage`的命令行工具前，确保在运行着`Ubuntu 16.04`
的服务器上，你拥有超级用户的权限或者你能够使用 `sudo` 命令，而且你已经在服务器上安装了`Oracle Virtual Box` 然后你需要安装
`VirtualBox`扩展包，这是运行`VRDE`远程桌面环境，访问无界面虚拟机所必须的。

### 2. 安装 VBoxManage

#### 1. 通过 [Virtual Box下载页]
(https://www.virtualbox.org/wiki/Downloads) 这个链接，你能够获取你所需要的软件扩展包的最新版本，扩展包的版本和你安装的
`VirtualBox` 版本需要一致！

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/VBoxManage/image1.png)

#### 2. 也可以用下面这条命令来获取 VBoxManage 扩展。
```bash
\$ wget http:*//download.virtualbox.org/virtualbox/5.0.20/Oracle_VM_VirtualBox_Extension_Pack-5.0.20-106931.vbox-extpack*
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/VBoxManage/image2.png)

#### 3. 运行下面这条命令，确认 VBoxManage 已经成功安装在你的机器上。
```bash
\$ VBoxManage list extpacks
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/VBoxManage/image3.png)

### 3. 在Ubuntu 16.04上使用VBoxManage

#### 1. 接下来我们将要使用 VBoxManage 向你展现通过命令行终端工具来新建和管理虚拟机是多么的简单。

运行下面的命令，新建一个将用来安装`Ubuntu`系统的虚拟机。
```bash
*\#* VBoxManage createvm \--name Ubuntu16.04 \--register
```
在运行了这条命令之后，`VBoxMnage`将会新建一个叫做`Ubuntu16.vbox`的虚拟机，这个虚拟机的位置是家目录路径下的 `VirtualBoxVMs/Ubuntu16/Ubuntu16.04.vbox`在上面这条命令中，`createvm`是用来新建虚拟机，`\--name` 定义了虚拟机的名字，而 `registervm`
命令是用来注册虚拟机的。

#### 2. 现在，使用下面这条命令为虚拟机创建一个硬盘镜像。
```bash
\$ VBoxManage createhd \--filename Ubuntu16.04 \--size 5124
```
这里，`createhd` 用来创建硬盘镜像，`\--filename`用来指定虚拟机的名称，也就是创建的硬盘镜像名称。`\--size`
表示硬盘镜像的空间容量，空间容量的单位总是`MB`。我们指定了`5Gb`也就是`5124 MB`。

#### 3. 接下来我们需要设置操作系统类型，如果要安装 Linux 系的系统
那么用下面这条命令指定系统类型为`Linux`或者`Ubuntu`或者`Fedora`之类的。
```bash
\$ VBoxManage modifyvm Ubuntu16.04 \--ostype Ubuntu
```
#### 4. 用下面这条命令来设置虚拟系统的内存大小，也就是从主机中分配到虚拟机系统的内存。
```bash
\$ VBoxManage modifyvm Ubuntu10.10 \--memory 512
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/VBoxManage/image4.png)

#### 5. 现在用下面这个命令为虚拟机创建一个存储控制器。
```bash
\$ VBoxManage storagectl Ubuntu16.04 \--name IDE \--add ide
\--controller PIIX4 \--bootable on
```
这里的 `storagect1` 是给虚拟机创建存储控制器的，`\--name`指定了虚拟机里需要创建、更改或者移除的存储控制器的名称。`\--add`
选项指明存储控制器所需要连接到的系统总线类型，可选的选项有` ide / sata / scsi / floppy``\--controller`
选择主板的类型，主板需要根据需要的存储控制器选择，可选的选项有 `LsiLogic / LSILogicSAS / BusLogic / IntelAhci / PIIX3 / PIIX4 / ICH6 / I82078`最后的 `\--bootable` 表示控制器是否可以引导系统。

上面的命令创建了叫做`IDE`的存储控制器。之后虚拟介质就能通过 `storageattach` 命令连接到该控制器。

#### 6. 然后运行下面这个命令来创建一个叫做 SATA 的存储控制器，它将会连接到之后的硬盘镜像上。
```bash
\$ VBoxManage storagectl Ubuntu16.04 \--name SATA \--add sata
\--controller IntelAhci \--bootable on
```
#### 7. 将之前创建的硬盘镜像和 CD/DVD 驱动器加载到 IDE 控制器。
将`Ubuntu`的安装光盘插到`CD/DVD`驱动器上。然后用 `storageattach` 命令连接存储控制器和虚拟机。
```bash
\$ VBoxManage storageattach Ubuntu16.04 \--storagectl SATA \--port 0
\--device 0 \--type hdd \--medium \`your_iso_filepath\`
```
这将把`SATA`存储控制器及介质（比如之前创建的虚拟磁盘镜像）连接到
`Ubuntu16.04`虚拟机中。

#### 8. 运行下面的命令添加像网络连接，音频之类的功能。
```
\$ VBoxManage modifyvm Ubuntu10.10 \--nic1 nat \--nictype1 82540EM
\--cableconnected1 on
```
```bash
\$ VBoxManage modifyvm Ubuntu10.10 \--vram 128 \--accelerate3d on
\--audio alsa \--audiocontroller ac97
```
### 4. 通过指定你想要启动虚拟机的名称，用下面这个命令启动虚拟机。
```bash
\$ VBoxManage startvm Ubuntu16.04
```
然后会打开一个新窗口，新窗口里虚拟机通过关联文件中引导。

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/VBoxManage/image5.png)

### 5. 你可以用接下来的命令来关掉虚拟机。
```bash
\$ VBoxManage controlvm Ubuntu16.04 poweroff
```
`controlvm` 命令用来控制虚拟机的状态，可选的选项有`pause / resume / reset / poweroff / savestate / acpipowerbutton / acpisleepbutton.controlvm `
### 6. 有很多选项，用下面这个命令来查看它支持的所有选项。
```bash
\$VBoxManage controlvm
```