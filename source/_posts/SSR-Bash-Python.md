---
title: SSR-Bash-Python安装脚本+教程/一键安装SSR+用户控制面板
author: liuliunx
coverImg: /medias/banner/5.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
 ' SSR-Bash-Python是Function Club开发的一款多用户管理脚本 拥有强大的功能,而且还有web面板功能,如果去售卖也是不错的选择,可以选择限制流量/带宽速度.'
tags:
  - Centos
  - SSR 
  - Python
categories:
  - 科学上网篇
abbrlink: d74d8b41
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---

### 1. 安装教程

#### 1. 安装脚本
```bash
> *wget http://www.gigsgigscloud.com/cn/downloads/ssr.sh && bash ssr.sh*
```
#### 2. 遇到输入\[y/n\]的时候全部输入` y `

#### 3. 安装成功后输入ssr进入控制面板先输入1在输入1启动服务然后回车输入2之后输入1添加用户

#### 4. 输入数字选择功能：
```bash
1.服务器控制

2.用户管理

3.全局流量管理

4.实验性功能

5.程序自检

请选择：` 2 `

1.添加用户

2.删除用户

3.修改用户

4.显示用户流量信息

5.显示用户名端口信息

直接回车返回上级菜单
```
请选择：` 1 `

#### 5. 你选择了添加用户

输入用户名： gigs（随便输入）

输入端口： 6666（随便输入）

输入密码： gigsgigs（随便输入）

#### 6. 加密方式（推荐7 但是也可以随便）
```bash
1.none

2.aes-128-cfb

3.aes-256-cfb

4.aes-128-ctr

5.aes-256-ctr

6.rc4-md5

7.chacha20

8.chacha20-ietf

9.salsa20
```
输入加密方式： ` 7 `

#### 7. 协议方式（推荐2 但是也可以随便）
```bash
1.origin

2.auth_sha1_v4

3.auth_aes128_md5

4.auth_aes128_sha1

5.verify_deflate

6.auth_chain_a

7.auth_chain_b
```
输入协议方式： ` 2 `

是否兼容原版协议（y/n）： ` y `

#### 8. 混淆方式
```bash
1.plain

2.http_simple

3.http_post

4.tls1.2_ticket_auth
```
输入混淆方式：` 2 `（推荐2/4 但是也可以随便）

#### 9. 是否兼容原版混淆（y/n）：

` y `

#### 10. 输入流量限制(只需输入数字，单位：GB)：

 `10000`（这个可以限制其他人流量）

#### 11. 是否开启端口限速（y/n）：

` n `（也可以输入y限速）
```bash
iptables: Saving firewall rules to /etc/sysconfig/iptables:\[ OK \]

iptables: Setting chains to policy ACCEPT: nat raw filter m\[ OK \]

iptables: Flushing firewall rules: \[ OK \]

iptables: Unloading modules: \[ OK \]

iptables: Applying firewall rules: \[ OK \]
```
#### 12. 用户添加成功！用户信息如下：
```bash
\### add user info
```
```bash
user : gigs

port : 6666

method : chacha20

passwd : gigsgigs

protocol : auth_sha1_v4_compatible

obfs : http_simple_compatible

transfer_enable : 10000.0 G Bytes

u : 0

d : 0

ssr://XX.X.X.X:6666:auth_sha1_v4:chacha20:http_simple:Z2lnc2dpZ3M

ssr://NDUuMzUuMTE5LjE1Mzo\*\*\*\*\*\*\*\*\*\*\*Ghfc2hhMV92NDpjaGFjaGEyMDpodHRwX3NpbXBsZTpaMmxuYzJkcFozTQ
```
`ShadowsocksR`服务器已启动

系统支持
```bash
> *Ubuntu 14*
>
> *Ubuntu 16*
>
> *CentOS 6*
>
> *CentOS 7*
>
> *Debian 7*
>
> *Debian 8（推荐）*
```
### 2. 功能介绍

一键开启、关闭SSR服务

添加、删除、修改用户端口和密码

自由限制用户端口流量使用

自动修改防火墙规则

自助修改SSR加密方式、协议、混淆等参数

自动统计，方便查询每个用户端口的流量使用情况

自动安装Libsodium库以支持Chacha20等加密方式

每月自动清空用户流量

一键封禁BT下载、垃圾邮件发送功能。（感谢逗比大佬提供）

#### 1. 脚本截图

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/SSR-Bash-Python/image1.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/SSR-Bash-Python/image2.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/SSR-Bash-Python/image3.png)


#### 2. 面板截图

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/SSR-Bash-Python/image4.png)

![](https://tpk1-1255836132.cos.ap-nanjing.myqcloud.com/SSR-Bash-Python/image5.png)
