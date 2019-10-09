# Python3

## 1. pip安装失败

报错关键信息如下：

```
There was a problem confirming the ssl certificate: Can't connect to HTTPS URL because the SSL module is not available
```

windows10下解决方案：

```
总是提示SSL有问题，然而只是SSL就在bin里边，所以没有生效。
Linux下一般需要安装openssl
Windows把安装目录下anaconda3\Library\bin 加入到系统环境变量后重启IDE或命令行即可。
主要参考：https://github.com/conda/conda/issues/6064
```

> 原文链接：win10下 pip install 后Can't connect to HTTPS URL because the SSL module is not available - cainiaochirou的博客 - CSDN博客  https://blog.csdn.net/cainiaochirou/article/details/90739310