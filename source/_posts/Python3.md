---
title: Python2.0与Python3.7之间代码版本问题
author: liuliunx
coverImg: /medias/banner/10.jpg
top: false
cover: false
toc: true
mathjax: false
summary: >-
  详细介绍Python2.0与Python3.7之间代码版本问题
tags:
  - Python
categories:
  - 运维篇
abbrlink: d74d8b57
reprintPolicy: cc_by
date: 2020-09-17 00:00:00
img: 
password:
---
### 一. `Python2.0`与`Python3.7`之间代码版本问题：

#### 1. 报错：`SyntaxError: Missing parentheses in call to \'print\'`

由于`python`的版本差异，造成的错误。

python2：
```bash
print \"hello python！\"
```
python3：
```bash
print (\"hello python!\")
```
`python2.0`的`raw_input`不能在python3.7中使用，必须修改为`input`才可以编译通过。

#### 2. 报错：[`invalidsyntax`](https://www.cnblogs.com/chongyou/p/5970992.html)

`python2.0`的`input`不带`（）`在`python3.7`中无法使用，必须修改为`input`带`（）`才可以编译通过。

#### 3. 报错：`TypeError: unsupported operand type(s) for \*: \'NoneType\' and\'float\'`

意思是：不支持此类操作`:`用`+`，\*等连接一个`NoneType`类型和`str`类型

`python2.0`的函数声明不带外围`（）`在`python3.7`中函数声明必须带外围`（）`，

`print (I - arr\[idx\]) \* rat\[idx\]`必须修改为`print ((I -arr\[idx\]) \* rat\[idx\])`才可以编译通过。

#### 4. 报错：`TypeError: \'\<=\' not supported between instances of \'str\'and \'int\'`

这是因为`input()`返回的数据类型是`str`类型，不能直接和整数进行比较，必须先把`str`转换成整型，使用`int()`方法：`I= int(input(\"pls input the lirun:\"))`

#### 5. 报错：`DeprecationWarning: the imp module is deprecated in favour ofimportlib; see the module\'s documentation for alternative uses`

`imp` 从 `Python 3.4` 之后弃用了，建议使用 `importlib` 代替

#### 6. 报错：`AttributeError: \'str\' object has no attribute \'decode\'`

`python3`的字符串默认为`unicode`格式(无编码)，因此字符串不用像`python2`一样显式地指出其类型，否则是语法错误。必须添加`encode(\'utf-8\')`的代码，不然字节字符串无法被程序解码。

`.encode(\'utf-8\').decode(\'utf-8\') `#必须将字节字符串解码后才能打印出来

#### 7. 报错：`FileNotFoundError: \[Errno 2\] No such file or directory`

原因是只添加`encode(\'utf-8\')，程序没有`解码成功，所以没有`找到要下载的目录，在.encode(\'utf-8\')后面必须添加.decode(\'utf-8\')，不然程序无法识别`字节字符串。

#### 8. 报错：`AttributeError: module \'urllib\' has no attribute\'urlretrieve\'`

`python2 `与`python3`的`urllib`不同在与`python3`要加上`.request`

比如：import urllib.request`

            res=urllib.request.urlopen(html)`

            data = urllib.request.urlretrieve(\"http://\...\")`

#### 9. 报错：`NameError: name \'reload\' is not defined`

注意： 

##### 1. `Python 3` 与 `Python 2` 有`很大的区别`，其中`Python3` 系统默认使用的就是utf-8编码。 

##### 2. 所以，对于使用的是`Python3` 的情况，就不需要`sys.setdefaultencoding(\"utf-8\")`这段代码。 

##### 3. `最重要的是`，`Python3` 的 `sys` 库里面已经没有` setdefaultencoding()` 函数了。

只要删除`import sys reload(sys) sys.setdefaultencoding(\"utf-8\")`就可以程序通过。或者：

对于` \<= Python 3.3：``import imp imp.reload(sys)`

对于` \>= Python 3.4：``import importlib importlib.reload(sys)`

#### 10. 运行某代码时，报错：`NameError:name 'xrange' is not defined`

原因：

在`Python 3`中，`range()`与`xrange()`合并为`range( )`。

我的`python`版本为`python3.5`。

解决办法：

将`xrange( )`函数全部换为`range( )`。
