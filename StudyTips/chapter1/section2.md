# 集成开发

## 1. 同时安装Python2和Python3

系统版本：CenterOS 7 64位 1810

安装版本：Python 3.6.9

安装方式：源代码安装

参考文档：Centos7安装python3并与python2共存  

转载地址：https://cloud.tencent.com/developer/news/228090

原文地址：https://kuaibao.qq.com/s/20180531G0D1B000?refer=cp_1026

​		CenterOS 7 默认安装了Python 2.7，因为再CenterOS中有些地方会用得到，但是目前主流版本为Python3.X，所以需要同时安装两个版本的Python。

### 1.1 查看Python版本

```
# python -V
Python 2.7.5
```

### 1.2 查看Python可执行文件位置

```
# which python
/usr/bin/python
```

### 1.3 查看Python软链

```
# ll python
lrwxrwxrwx 1 root root 7 Jul 11 10:58 python -> python2
```

### 1.4 安装依赖库

```
# yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
```

### 1.5 备份软链

```
# mv python python.bak
```

### 1.6 下载编译安装包

```
# wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tar.xz
```

### 1.7 解压安装包

```
# tar -xvJf Python-3.6.9.tar.xz
```

> **注意**：`-xvJf`中第三个字母`J`为大写

### 1.8 编译

```
# cd Python-3.6.9
# ./configure prefix=/usr/local/python3
```

> **注意**：`./`之后不需要空格

### 1.9 安装

```
# make
......
# make install
```

​		安装完成后，`/usr/local/`目录下会多出一个`python3`的目录。

### 1.10 添加软链

```
# ln -s /usr/local/python3/bin/python3 /usr/bin/python
```

### 1.11 测试

```
# python -V
Python 3.6.9
# python2 -V
Python 2.7.5
```

### 1.12 修改其他配置

```
# vi /usr/bin/yum
```

​		将`#!/usr/bin/python`修改为`#!/usr/bin/python2`

```
# vi /usr/libexec/urlgrabber-ext-down 
```

​		将`#!/usr/bin/python`修改为`#!/usr/bin/python2`

