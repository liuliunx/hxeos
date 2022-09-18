---
title: lnmp全面优化
author: liuliunx
coverImg: /medias/banner/10.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
  优化nginx,mysql,PHP配置信息
tags:
  - Centos
  - 服务器系统环境搭建教程 
  - LNMP
categories:
  - 运维篇
abbrlink: d74d8b52
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---

## 一：lnmp的nginx优化

主要是修改 `/usr/local/nginx/conf/nginx.conf`

### 1.`lnmp`安装包中

`nginx`的`worker_processes`默认设置是`1`，这里我们要根据服务器`cpu`具体的核心数来优化。通常`4`核的`CPU`我会把值设为`3`。

```bash
  1 
  2  2核CPU，开启2个进程 worker_processes 2; worker_cpu_affinity 01 10;
  3 
  4  4核CPU，开3个进程 worker_processes 3; worker_cpu_affinity 0010
  5 
  6  0100 1000;   8核CPU，开8个进程 worker_processes 8;
  7 
  8  worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000
  9 
  10 00100000 01000000 10000000;
  11   
```

　`worker_processes`参数解析可参考：[worker_processes详解](http://www.cnblogs.com/sunbeidan/p/5016714.html)

### 2.`worker_rlimit_nofile`参数默认是`5xxxx`.

```bash
  1 
  2 worker_rlimit_nofile 65535;   events     {         use epoll;
  3 
  4         worker_connections 32700;     }
  5 
  6 
  7   
```

`worker_rlimit_nofile`参数讲解可参考：[nginx优化参数详解](http://www.cnblogs.com/sunbeidan/p/5016702.html)

### 3. 添加防压力测试
```bash
if (\$http_user_agent \~
ApacheBench\|WebBench\|Jmeter\|must-revalidate\|Havij) {retun 503;}
```
### 4. 添加针对`CVE-2013-4547`链接空格的补丁
```bash
if (\$request_uri \~ \" \") {return 444;}
```
## 二：`lnmp`的`mysql`优化

用`/usr/local/mysql/share/mysql/`目录下的`my-large.cnf`文件替换根目录`etc`下的`my.cnf`文件
```bash
my-huge.cnf: 适合1GB - 2GB RAM主机使用

my-large.cnf: 适合 512MB RAM使用

my-medium.cnf: 只有 32MB - 64MB RAM使用

my-small.cnf:小于64MB 用，MySQL会占用很少资源

my-innodb-heavy-4G.cnf 适合4G以上使用
```
```bash
  1 
  2 禁用mysql日志： 修改 /etc/my.cnf 文件
  3 
  4 在log-bin=mysql-bin和binlog_format=mixed 这两行前面加#注释掉即可。
  5 
  6   在query_cache_size= 16M下面添加一行： tmp_table_size = 200M
  7   
```

`mysql`参数讲解可参考：[mysql优化](http://www.cnblogs.com/sunbeidan/p/5016577.html)　　

## 三：`lnmp`的`php`相关参数优化

优化主要是修改`/usr/local/php/etc/`目录下的`php-fpm.conf`和`php.ini`文件

### 1. `php-fpm.conf`参数优化

删除`value name=\display_errors\`这一行的代码，防止坏人从PHP错误中找到漏洞。

`max_children`默认参数是开启5个进程。数值要根据内存大小来定，每一个php-cgi所耗费的内存在20M左右。

```bash
  1
  2 126M内存默认即可 256M 10个 512M 20个 1G 40个
  3 
  4 
```

`request_terminate_timeout`参数默认是`0s`，修改为`300s`

`rlimit_files`参数默认`5xxxx`，修改为`65535`

`php-fpm`参数讲解可参考：[php-fpm详解](http://www.cnblogs.com/sunbeidan/p/5016657.html)

### 2. `php.ini`参数优化

`disable_functions =`
默认禁用了一些参数，PHP中有一些函数的风险性还是相当大的,如果允许这些函数执行,当`PHP`
程序出现漏洞时,损失是非常严重的

```bash
  1 
  2 fsockopen这个参数用的比较多，可以删除。  
  3 
  4 另外从安全方面考虑可隐藏PHP版本号 将文件里面的 expose_php = On
  5 
  6 修改为 expose_php = Off 即可   将display_errors =On改为Off
  7   
```

最后修改最大连接数使重启后也可生效，在`/etc/profile` 最后增加一行 `ulimit-SHn 65535`

另外`LNMP`安装包里有一个`eAccelerator`的安装文件。最好装一下。这个是加速`PHP`缓存的还不错。

关于`eAccelerator`的设置我就给出两个修改的地方吧：

```bash
  1   eaccelerator.shm_size=\"16\"

```

默认是占用`16M`共享内存，要是`1`，你就改成`16`吧。大小也可根据你的内存情况设置。

 

另外军哥默认是`eaccelerator`缓存目录是`/usr/local/eaccelerator_cache`，这样用硬盘缓存的话，某些情况会影响`php`的响应时间。我们可以直接放到共享内存里面老。
```bash
运行命令：mkdir -p /dev/shm/eaccelerator_cache
```
修改目录为以下就`OK`了。

```bash
  1   eaccelerator.cache_dir=\"/dev/shm/eaccelerator_cache\"
```

## 最后全部修改完记得重启生效：
```bash
lnmp restart
```