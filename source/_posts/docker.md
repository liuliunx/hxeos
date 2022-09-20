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
本文是在我查看了很多前辈的博客上完成的有很多借阅的成分，主要记录`docker`从安装到部署`Javaweb`程序的整个过程，希望对有需要的人有所帮助，我是个菜鸟，望多多包涵。
### 一. CentOS 07 Docker 安装
使用`Ctrl+alt`在虚拟机和`Windows`切换鼠标，直接复制文档里的指令。
安装了图形化界面：
在虚拟机中右键粘贴即可
没安装图形化界面：切换鼠标光标在虚拟机命令行按一下`Ctrl+alt`之后按`Ctrl+V`。
 
前提条件：
`Docker` 运行在`CentOS 7 `上，要求系统为`64`位、系统内核版本为` 3.10 `以上。
#### 1. 使用 `yum` 安装`docker`（`CentOS 7`下）：
`Docker` 要求 `CentOS` 系统的内核版本高于 `3.10`
#### 2. 首先通过 `uname -r` 命令查看你当前的内核版本
```bash
uname -r
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image1.png)
#### 3. 使用`su root`切换用户权限到`root`
```bash
 su root
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image2.png)
输入你`root`权限的密码（密码是你安装虚拟机时设置的超级管理密码），密码正确如上图进入`root`权限
#### 4.使用
```bash
yum update
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image3.png)
确保 `yum` 包更新到最新。(备注：以下指令执行时会弹出很多内容，只截取了一部分图片，需要一定时间，等待即可)，出现统计信息提示是否确认，输入`y`并按下`Enter`。
 
#### 5. 卸载旧版本(如果安装过旧版本的话，没安装过直接跳过)
```bash
yum remove docker docker-common docker-selinux docker-engine
```
#### 6. 安装一些必要的系统工具：
```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image4.png)
#### 7. 添加软件源信息：
```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image5.png)
#### 8. 安装 docker-ce：
```bash 
yum -y install docker-ce
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image6.png)
#### 9. 启动并加入开机启动
```bash
 systemctl start docker
 systemctl enable docker
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image7.png)
#### 10. 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
```bash
docker version
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image8.png)
如果失败：重新执行第`5`步，接着执行第`8`步
  
### 二. docker安装mysql将数据挂在宿主机
#### 1.在虚拟机创建本地目录（/是指在根目录下）
```bash
mkdir /data
mkdir /data/MySQL
mkdir /data/MySQL/datadir
```
由于本人已经建好目录所以显示已存在
 ![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image9.png)
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image10.png)
新建三个文件夹，层级关系，我们最后把存储路径放到`datadir`下，同样的我们在`MySQL`文件夹中新建一个`conf.d`文件夹放配置文件：
```bash
kdir /data/MySQL/conf.d
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image11.png)
#### 2. 新建运行并取名为一个mysql5.7的docker服务
 把端口映射到`3306` `MySQL`存储路径放到新建的`datadir`文件夹中，把配置文件放到`conf.d`中（这里的`/var....` 和`/etc ....`是`docker`服务默认位置，一重启就没了，所以我们要换掉），定义`root`账户密码为`123456`， 运行镜像是`MySQL：5.7`
```bash
docker run --name mysql5.7 -p 3306:3306 -v/data/MySQL/datadir:/var/lib/mysql -v /data/MySQL/conf.d:/etc/mysql/conf.d -eMYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image12.png)
（这里省略了`MYSQL：5.7`的镜像下载，但执行`run`时，如果本地没有该镜像，会从公共镜像库里下载你指定版本的镜像，若不指定：自动默认下载镜像库里`latest`版本的镜像）
 
#### 3. 进入容器内部：
```bash
docker exec -it mysql5.7 /bin/bash
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image13.png)
#### 4. 进入mysql命令行
```bash
mysql –uroot –p
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image14.png)
密码是你创建`mysql5.7`时设置的密码，忘记时，回看第`2`步 `PASSWORD=123456`

这里密码是不可见得，直接输入密码按回车即可
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image15.png)
#### 5.现在你可以创建项目需要的数据库和相关的表了。
 
 
### 三. docker安装tomcat
#### 1.新建并运行一个名为`tomcat01`的服务
把端口映射到`8080`，连接到刚才创建数据库服务器的容器`mysql5.7` 
```bash
docker run -d -p 8080:8080 --name tomcat01 --link mysql5.7 -d tomcat[b1]
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image16.png)
看到一长串`ID`编号表示`tomcat`新建成功

#### 2．在浏览器输入虚拟机ip地址+：tomcat端口号8080
如：`192.168.227.129:8080`
虚拟机`ip`：名称为`ens33`对应的`ip`地址
```bash
ip address           #（查询IP地址）
ifconfig ens33       #(查询ens33的ip地址)
``` 
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image17.png)
如果安装成功：会弹出如下界面:
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image18.png)
### 四. 部署Javaweb项目
 
#### 0. 前提条件修改项目里有关连接mysql的URL以及配置文件的信息
```bash
 String url1="jdbc:mysql://mysql5.7:3306/book";
 String url2="?user=root&password=123456";
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image19.png)
如上面：`mysql5.7`是我在`docker`中运行的容器名（这里也可以写`mysql5.7`的`ip`但不推荐，因为`ip`会变，而容器名称是你自己创建容器时命名的），这里的`root`是`mysql`的账户名，密码是你创建该容器时设置的密码，这里是`123456`。
至于`war`文件的生成这里就不详细介绍了。
#### 1.  在根目录下新建文件夹webapps(用来存放.war文件)
```bash
mkdir  /webapps
```
#### 2. 通过U盘将目标文件拷贝到根目录下的webapps
选中虚拟机，鼠标右键点击选择可移动设备，选择你的`U`盘，选择断开连接（连接主机）（D）（备注：可能需要再次操作一遍，最后一步变成连接（连接主机）（D））之后就可以直接复制你优盘里的目标文件到根目录下的`webapps`里。
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image20.png)
#### 3. 将根目录下webapps里的目标文件复制到tomcat
（备注：首先从下面指令`2`开始执行一直到指令4可以看到该路径下没有目标文件（注意：部署web项目时除了目标文件，该路径下没有其他文件，若有请使用`rm –f `文件全称，删除文件）
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image21.png)
按下`Ctrl+d` 或者输入`exit`退出容器
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image22.png)
这时从指令`1`进行操作
```bash
docker cp /webapps/bookManagement.war tomcat01:/usr/local/tomcat/webapps
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image23.png)
```bash
docker exec -it tomcat01 /bin/bash      #（进入容器tomcat01内部）
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image24.png)
```bash
cd webapps      #（进入/usr/local/tomcat/webapps路径下）
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image25.png)
```bash
ls       #（查看webapps文件夹里的文件）此时目标文件bookManagement已在/usr/local/tomcat/webapps路径下
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image26.png)
#### 4. 复制成功后，重新启动tomcat01容器，会自动完成目标文件的部署（备注：启动tomcat01之前确保mysql5.7是启动的，由于tomcat01连接了mysql5.7）
```bash
docker restart tomcat01     #（重启tomcat01容器）
```
至此一个`Javaweb`项目就在`docker`上部署成功了。

#### 5. 打开浏览器在地址栏输入虚拟机地址：8080/项目名称就可以访问了（备注如果这里不知道虚拟机ip地址是什么请回看3.docker安装tomcat的第2步）
例如：`192.168.227.131:8080/bookManagement`
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/docker/image27.png)

### 四. 备注：由于虚拟机地址会变，解决方法
#### 1. 在使用虚拟机`ip`前，使用
```bash
ip address # 查看ens33对应的ip地址
```
或是使用
```bash
ifconfig ens33 # 查看ens33对应的ip地址
``` 
#### 2. 将虚拟机的`ip`设置为静态`ip`
`Docker` 常用指令：
```bash
 docker images        # 查看本地镜像

docker ps -a         # 查看所有容器

docker ps            # 查看当前有哪些容器正在运行

docker rmi # 镜像名称/镜像ID    删除镜像

docker rm # 容器名称/容器ID     删除容器（删除前必须先停止容器的运行）

docker start # 容器名称/容器ID      启动一个容器

docker restart # 容器名称/容器ID     重启一个容器

docker stop # 容器名称/容器ID     停止一个在运行的容器

docker run -d -p 8081:8080 --name tomcat01 tomcat # 利用镜像创建一个容器

-d: # 在后台运行

-p: # 映射端口号 这里将tomcat01的端口8080映射到宿主机的8081端口

--name: # 为容器取名字

tomcat: # 本地镜像仓库的镜像 

ctrl+d # 退出容器且关闭,

ctrl+p+q # 退出容器但不关闭,

docker exec -it # 容器名称/容器ID /bin/bash：进入容器
```

