---
title: tar.gz解压缩到指定目录
tags: 
  - shell
  - linux
  - 学习笔记
date: 2018-10-12 16:35:27
categories: shell
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw4acxk758j30i20i2mxc.jpg
---

# tar.gz包解压到指定目录

​	众所周知，将一个`tar.gz`的压缩包解压的命令是：

```shell
tar -zxvf xxxx.tar.gz
```

​	这样是将压缩包解压缩到了当前目录。

如果要解压到指定目录，那么就加`-C`参数：

范例：将`abc.tar.gz`解压到`path-a`目录

```shell
tar -zxvf abc.tar.gz -C path-a
```

