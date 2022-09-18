---
title: 安装项目在docker环境配置
author: liuliunx
coverImg: /medias/banner/10.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
  docker(包括docker、mysql、tomcat的安装，以及部署web工程文件)
tags:
  - CentOS
  - docker
  - mysql
  - tomcat
  - web
categories:
  - 运维篇
abbrlink: d74d8b58
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---
### 前言
本文是在我查看了很多前辈的博客上完成的有很多借阅的成分，主要记录docker从安装到部署Javaweb程序的整个过程，希望对有需要的人有所帮助，我是个菜鸟，望多多包涵。
### 一. CentOS 07 Docker 安装
使用Ctrl+alt在虚拟机和Windows切换鼠标，直接复制文档里的指令。
安装了图形化界面：
在虚拟机中右键粘贴即可
没安装图形化界面：切换鼠标光标在虚拟机命令行按一下Ctrl+alt之后按Ctrl+V。
 
前提条件：
Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。
1.使用 yum 安装docker（CentOS 7下）：
Docker 要求 CentOS 系统的内核版本高于 3.10
1.首先通过 uname -r 命令查看你当前的内核版本
 
2.使用su root切换用户权限到root
 
 
输入你root权限的密码（密码是你安装虚拟机时设置的超级管理密码），密码正确如上图进入root权限
3.使用yum update确保 yum 包更新到最新。(备注：以下指令执行时会弹出很多内容，只截取了一部分图片，需要一定时间，等待即可)，出现统计信息提示是否确认，输入y并按下Enter。
 
4.卸载旧版本(如果安装过旧版本的话，没安装过直接跳过)
5.安装一些必要的系统工具：
 
6.添加软件源信息：
 
7.安装 docker-ce：
 
 8.启动并加入开机启动
 
 
9.验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
 
 
如果失败：重新执行第4步，接着执行第7步
  
2.docker安装mysql将数据挂在宿主机
1.在虚拟机创建本地目录（/是指在根目录下）
由于本人已经建好目录所以显示已存在
 
 
新建三个文件夹，层级关系，我们最后把存储路径放到datadir下，同样的我们在MySQL文件夹中新建一个conf.d文件夹放配置文件：
 
 
 
  
2.新建运行并取名为一个mysql5.7的docker服务 把端口映射到3306 MySQL存储路径放到新建的datadir文件夹中，把配置文件放到conf.d中（这里的/var.... 和/etc ....是docker服务默认位置，一重启就没了，所以我们要换掉），定义root账户密码为123456， 运行镜像是MySQL：5.7
 
（这里省略了MYSQL：5.7的镜像下载，但执行run时，如果本地没有该镜像，会从公共镜像库里下载你指定版本的镜像，若不指定：自动默认下载镜像库里latest版本的镜像）
 
 
 
3.进入容器内部：
 
4.进入mysql命令行
 
 
密码是你创建mysql5.7时设置的密码，忘记时，回看第2步（PASSWORD=123456）PASSWORD=123456
这里密码是不可见得，直接输入密码按回车即可
  
5.现在你可以创建项目需要的数据库和相关的表了。
 
 
3.docker安装tomcat
1.新建并运行一个名为tomcat01的服务 把端口映射到8080，连接到刚才创建数据库服务器的容器mysql5.7
 
如图看到一长串ID编号表示tomcat新建成功
2．在浏览器输入虚拟机ip地址+：tomcat端口号8080
如：192.168.227.129:8080
虚拟机ip：名称为ens33对应的ip地址
 
如果安装成功：会弹出如下界面
 
 
 
 
4.部署Javaweb项目
 
前提条件修改项目里有关连接mysql的URL以及配置文件的信息
 
 
如上图：mysql5.7是我在docker中运行的容器名（这里也可以写mysql5.7的ip但不推荐，因为ip会变，而容器名称是你自己创建容器时命名的），这里的root是mysql的账户名，密码是你创建该容器时设置的密码，这里是123456。
至于war文件的生成这里就不详细介绍了。
1.  在根目录下新建文件夹webapps(用来存放.war文件)
 
2.通过U盘将目标文件拷贝到根目录下的webapps
如图选中虚拟机，鼠标右键点击选择可移动设备，选择你的U盘，选择断开连接（连接主机）（D）（备注：可能需要再次操作一遍，最后一步变成连接（连接主机）（D））之后就可以直接复制你优盘里的目标文件到根目录下的webapps里。
 
 
3.将根目录下webapps里的目标文件复制到tomcat
（备注：首先从下面指令2开始执行一直到指令4可以看到该路径下没有目标文件（注意：部署web项目时除了目标文件，该路径下没有其他文件，若有请使用rm –f 文件全称，删除文件）
 
按下Ctrl+d 或者输入exit退出容器
 
这时从指令1进行操作
 
 
  
 
 
4.复制成功后，重新启动tomcat01容器，会自动完成目标文件的部署（备注：启动tomcat01之前确保mysql5.7是启动的，由于tomcat01连接了mysql5.7）
至此一个Javaweb项目就在docker上部署成功了。
 
5.打开浏览器在地址栏输入虚拟机地址：8080/项目名称就可以访问了（备注如果这里不知道虚拟机ip地址是什么请回看3.docker安装tomcat的第2步）
例如：192.168.227.131:8080/bookManagement
 
 
备注：由于虚拟机地址会变，解决方法
1.  在使用虚拟机ip前，使用
或是使用
 
2.将虚拟机的ip设置为静态ip
Docker 常用指令：
 
 [b1]



  指令：uname --r




  指令：su root



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image1.png)

  指令：yum update



![2-1076578365.png](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image2.png)


  指令：yum remove docker docker-common docker-selinux docker-engine




  指令：yum install -y yum-utils device-mapper-persistent-data lvm2



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image3.png)


  指令：yum-config-manager
  \--add-repo <http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo>



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image4.png)


  指令：yum -y install docker-ce



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image5.png)


  指令：systemctl start docker 指令：systemctl enable docker



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image6.png)


  指令：docker version



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image7.png)


  指令：mkdir /data 指令：mkdir /data/MySQL 指令：mkdir
  /data/MySQL/datadir



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image8.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image9.png)


  指令：mkdir /data/MySQL/conf.d



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image10.png)


  指令：docker run \--name mysql5.7 -p 3306:3306
  -v/data/MySQL/datadir:/var/lib/mysql -v
  /data/MySQL/conf.d:/etc/mysql/conf.d -eMYSQL_ROOT_PASSWORD=123456 -d
  mysql:5.7



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image11.png)


  指令：docker exec -it mysql5.7 /bin/bash



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image12.png)


  指令：mysql --uroot --p



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image13.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image14.png)


  指令：docker run -d -p 8080:8080 \--name tomcat01 \--link mysql5.7 -d
  tomcat



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image15.png)


  指令：ip address              （查询IP地址）或 指令：ifconfig
  ens33          (查询ens33的ip地址)



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image16.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image17.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image18.png)


  指令：mkdir  /webapps



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image19.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image20.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image21.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image22.png)


  指令1: docker cp /webapps/bookManagement.war
  tomcat01:/usr/local/tomcat/webapps



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image23.png)


  指令2：docker exec -it tomcat01 /bin/bash     
  //（进入容器tomcat01内部）



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image24.png)

```bash
  指令3：cd webapps      //（进入/usr/local/tomcat/webapps路径下）

```

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image25.png)

```bash
  指令4：ls  

```
      //（查看webapps文件夹里的文件）此时目标文件bookManagement已在/usr/local/tomcat/webapps路径下



![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image26.png)

```bash
  指令5：docker restart tomcat01     //（重启tomcat01容器）

```


![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image27.png)

```bash
  指令：ip address 查看ens33对应的ip地址
```
```bash
  指令：ifconfig ens33 查看ens33对应的ip地址
```
```bash
docker images        //查看本地镜像
docker ps -a         //查看所有容器
docker ps            //查看当前有哪些容器正在运行
docker rmi 镜像名称/镜像ID    删除镜像
docker rm 容器名称/容器ID     删除容器（删除前必须先停止容器的运行）
docker start 容器名称/容器ID      启动一个容器
docker restart 容器名称/容器ID     重启一个容器
docker stop 容器名称/容器ID     停止一个在运行的容器
docker run -d -p 8081:8080 --name tomcat01 tomcat 利用镜像创建一个容器
-d: 在后台运行
-p:映射端口号 这里将tomcat01的端口8080映射到宿主机的8081端口
--name: 为容器取名字
tomcat: 本地镜像仓库的镜像 
ctrl+d 退出容器且关闭,
ctrl+p+q 退出容器但不关闭,
docker exec -it 容器名称/容器ID /bin/bash：进入容器
```
