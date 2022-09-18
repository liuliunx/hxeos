---
title: 戴尔服务器PERC6i控制器配置raid5
author: liuliunx
coverImg: /medias/banner/9.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
  RAID5和RAID4一样，数据以块为单位分布到各个硬盘上。RAID 5不对数据进行备份，而是把数据和与其相对应的奇偶校验信息存储到组成RAID5的各个磁盘上，并且奇偶校验信息和相对应的数据分别存储于不同的磁盘上。当RAID5的一个磁盘数据损坏后，利用剩下的数据和相应的奇偶校验信息去恢复被损坏的数据。
tags:
  - RAID5
  - 服务器系统环境搭建教程 
  - DELL
categories:
  - 运维篇
abbrlink: d74d8b42
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---
**戴尔服务器PERC6i控制器配置raid5**

###  首先启动服务器

#### 1. 出现`Pressto Run Configuration Utility`提示时，按下`ctrl+r`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image1.png)
 

#### 2. 出现如下图所示界面，把光标移到虚拟磁盘上，可以看到当前4块硬盘已经做成了`RAID5`


![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image2.png)

#### 3. 这里，它不符合我们三块硬盘做RAID5，
一块硬盘做热备的要求，要把默认做好的`RAID5`删除，重新做。光标移动到`VirtualDisk 0`选择虚拟磁盘，按F2键操作，然后选择`delete VD`删除虚拟磁盘 


![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image3.png)

#### 4. 下面是删除之后的界面，你会发现什么都没了. 
 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image4.png)


#### 5. 在`No Configuration Present`上按`F2`键，选择`Create NewVD`创建新的虚拟磁盘 


![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image5.png)

#### 6. 下面是创建新虚拟磁盘的界面 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image6.png)

#### 7. 在`RAID`级别中选择`RAID5`，然后勾选择三个磁盘，OK确定 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image7.png)

#### 8. 从下图我们可以看到三块硬盘做成了一个虚拟盘 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image8.png)

#### 9. 我们按`ctrl+N`换到下一个页面，看到还有最后一块磁盘没有用到 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image9.png)

#### 10. 我们把光标移到最后一块硬盘上，按F2键，选择`Make Global HS`，然后确定 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image10.png)

#### 11. 返回首页面，我们看到了热备份已经有了 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image11.png)

#### 12. 最后一步，`RAID`的初始化，光标移到虚拟磁盘上，按`F2`选择`Initialzation``StartInit.`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/raid5/image12.png)

由于初始化花费时间太长，所以没有进行这一步操作。

这样`RAID5`策略就做完了。

下边是安装`centos6.5`系统，由于使用`BIOS`进行引导系统安装要求磁盘不能大于`2T`,所以是使用U盘安装。

开机直接狂点`F11`，进入后进入`one-short-boot`（大概是这个）选择`Disk Usb`
启动，然后就进入了安装界面，然后就一步步安装就`OK`啦。
