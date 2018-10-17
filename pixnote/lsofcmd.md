---
title: lsof命令查询端口占用和文件占用
tags:
  - shell
  - linux
  - 学习笔记
date: 2018-10-11 14:42:58
categories: shell
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw4acxk758j30i20i2mxc.jpg
---

# lsof命令查询端口占用和文件占用

参考：https://www.cnblogs.com/onmyway20xx/p/4425330.htm

## 1、端口占用

`lsof`工具查询端口占用非常简单

**范例：**查询`8899`占用情况

```shell
$ lsof -i:8899
COMMAND  PID          USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
java    1152 tangpengxiang   26u  IPv6 0xceff0736a28febab      0t0  TCP *:8899 (LISTEN)
```



## 2、查询文件占用

这种情况适用于U盘卸载不掉被占用的情况

范例：

```shell
lsof /Volumes/Data
```

