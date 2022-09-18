---
title: Centos 6.9安装 Openvpn
author: liuliunx
coverImg: /medias/banner/12.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
  OpenVPN 是一个基于 OpenSSL 库的应用层 VPN 实现。和传统 VPN 相比，它的优点是简单易用。
tags:
  - Centos
  - Openvpn 
  - 服务器系统环境搭建教程
categories:
  - 运维篇
abbrlink: d74d8b38
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---
# Centos 6.9安装 Openvpn

** **

## 0x01  安装`openvpn  server`

### 1. 下载安装脚本

```bash
wget https://git.io/vpn -O openvpn-install.sh
yum install -y vsftpd #安装FTP服务
```
 

### 2. `vi openvpn-install.sh`

修改脚本，注掉脚本中的 
```bash
setenv opt block-outside-dns
```
并添加一行
```bash
reneg-sec 0
```

### 3. 执行安装

```bash 
bash  openvpn-install.sh
```

### 4. 填写内网ip地址

**直接回车**

### 5. 添加外网ip地址或者域名

## 0x02 中枢服务器IP

### 1. 选择TCP或者UDP协议，选择1

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image1.jpeg)
 
 
### 2. 填写vpn服务器端口，默认填写`17173`

 

### 3. 选择`dns`服务器，默认选择1

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image2.jpeg)
 

### 4. 填写`client`证书名称，填写节点服务器的MAC

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image3.jpeg)
 

#### 5. 配置完成，按`any key`继续执行

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image4.jpeg)

### 6. 修改`server`配置文件`/etc/openvpn/server.conf`，注释`push`
\"redirect-gateway def1 bypass-dhcp\"，如下图

```bash
 vi /etc/openvpn/server.conf
```

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image5.jpeg)
 

### 7. 安装成功，`client`的配置文件`/root/client.ovpn`

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image6.jpeg)
 

### 8. 自启动

```bash
   echo \"/etc/init.d/openvpn  start \"  \>\> /etc/rc.local
```

### 查询 `openvpn`服务有没有启动

```bash
ps -ef \| grep openvpn
```

## 问题：

### 1.检测`openvpn`进程是否启动，如果服务器没有开启，则执行

```bash
sudo /etc/init.d/openvpn restart
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image7.jpeg)

 

### 2.连接上`openvpn`服务器，但是无法访问网页或者外网，检测`/etc/sysctl.conf`文件`net.ipv4.ip_forward`选项，设置
```bash
net.ipv4.ip_forward = 1
```
然后执行
```bash
sysctl --p
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image8.jpeg)
 

## 0x03  安装`openvpn  client`

### 1. 安装`openvpn`软件
```bash
yum install openvpn -y
```
### 2. 拷贝`openvpn`服务器端的`/root/client.ovpn`节点服务器的MAC文件，放在`/etc/openvpn`目录下

### 3. 启动`client.ovpn`的名要修改节点服务器的MAC的名
```bash
openvpn \--daemon \--cd /etc/openvpn \--config client.ovpn \--log-append
/var/log/openvpn.log
```
### 4. 自启动`client.ovpn`的名要修改节点服务器的MAC的名
```bash
 echo \"openvpn \--daemon \--cd /etc/openvpn \--config client.ovpn
\--log-append /var/log/openvpn.log\" \>\> /etc/rc.local
```
 

## 0x04  `openvpn`服务器设置客户端固定`ip`

### 1. 在服务端配置文件`/etc/openvpn/server.conf`中设置
```bash
client-config-dir /etc/openvpn/ccd

vi /etc/openvpn/server.conf
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image9.jpeg)
 

### 2. 创建`/etc/openvpn/ccd`目录

 ```bash
mkdir -p  /etc/openvpn/ccd
```

### 3. cdd文件夹中的文件为对应客户端所使用的登录名称,即安装`openvpn server`
### 的`client name`时候填写名称`client(client证书名称或用户名称)`。在文件client填写

```bash
ifconfig-push 10.8.0.2 255.255.255.0
```

这设置可配置使用client帐号登录的客户端ip地扯为`10.8.0.5`如图

`vi /etc/openvpn/ccd/`节点服务器的MAC的名

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image10.jpeg)


### 4. 重启`openvpn`服务
```bash
/etc/init.d/openvpn restart
```


## 0x05  openvpn服务器申请客户端证书

### 1.  执行脚本文件`openvpn-install.sh`
```bash
bash openvpn-install.sh
```
![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image11.jpeg)
 

### 2. 选择功能1

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image12.jpeg)
 

### 3. 填写客户端证书名称（`client name`），即客户端用户名称

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image13.jpeg)
 

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image14.jpeg)

 

### 4. 生成用户名称john的客户端证书`/root/john.ovpn`文件

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/openvpn/image15.jpeg)

## 0x06 安装pip:
```bash
  yum install python-pip
```
## 0x07 安装**shadowsocks**
```bash
 pip install shadowsocks
```
### 1. 配置客户端信息：
```bash
vi /etc/shadowsocks.json
```
```bash
{

> \"server\":\"0.0.0.0\",
>
> \"server_port\":17175,
>
> \"local_address\": \"127.0.0.1\",
>
> \"local_port\":1080,
>
> \"password\":\"ed55f40ca39fcee9\",
>
> \"timeout\":300,
>
> \"method\":\"aes-256-cfb\",
>
> \"fast_open\": false

}
```
前台运行(`Ctrl+C`或者关闭终端服务会自动停止)：
```bash
 ssserver -c /etc/shadowsocks.json -d start
```
后台运行**(推荐**，关闭终端后服务会继续运行)：
```bash
 ssserver -c /etc/shadowsocks.json -d restart 
```
查询 `ssserver`服务有没有启动
```bash
ps -ef \| grep ssserver
```
### 2. 备注:

端口转发规则

`vi /root/ipcloud/firewall.sh`
```bash
iptables -F

iptables -t nat -F

iptables -P INPUT ACCEPT

iptables -P FORWARD ACCEPT

iptables -t nat -A PREROUTING  -p tcp  -i eth0  \--dport 10082 -j DNAT
\--to-destination 10.8.0.2:17175

iptables -t nat -A PREROUTING  -p udp  -i eth0  \--dport 10082 -j DNAT
\--to-destination 10.8.0.2:17175

iptables -t nat -A POSTROUTING -p tcp -d 10.8.0.2/24 \--dport 17175 -j
SNAT \--to-source 10.8.0.1

iptables -t nat -A POSTROUTING -p udp -d 10.8.0.2/24 \--dport 17175 -j
SNAT \--to-source 10.8.0.1
```
注意要修改10082为节点服务器ID号
