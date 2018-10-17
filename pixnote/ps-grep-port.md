---
title: ps命令取出pid
tags:
  - shell
  - linux
  - 学习笔记
date: 2018-10-16 16:38:10
categories: shell
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw4acxk758j30i20i2mxc.jpg
---

# ps命令获取对应的pid及其余信息

参考：https://blog.csdn.net/wenxuechaozhe/article/details/69083476

本文主要介绍在项目使用当中对ps命令的使用总结（本文以zookeeper为例进行说明）

- 1、首先使用ps -ef 命令能够得到当前系统运行的所有进程信息

```ruby
ps -ef 
```

- 2、查找想要获取的进程信息，例zookeeper

```ruby
ps -ef  |grep zookeeper
```

- 3、忽略grep查询进程名

```ruby
ps -ef | grep zookeeper | grep -v grep
```

- 4、进行查找该进程的PID，执行操作

```ruby
ps -ef | grep zookeeper | grep -v grep | awk '{print $2}'
```

- 5、获取到pid后，可以进行后续处理。比如执行删除等操作。

```ruby
ps -ef | grep zookeeper | grep -v grep | awk '{print $2}' | xargs kill -9
```

