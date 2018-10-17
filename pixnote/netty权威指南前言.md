---
title: (学习netty)netty权威指南前言
categories: netty
tags:
  - netty
  - socket
  - java
  - 学习笔记
date: 2018-10-10 14:56:08
cover_picture: https://ws4.sinaimg.cn/large/006tNbRwgy1fw447bl60pj308u04kgli.jpg
---

# 前言及java通讯历史简介、

[《Netty权威指南第２版》　作者李林峰]

2008年，项目BBS:同步阻塞式HTTP+XML进行通信，性能缓处理缓慢。

故障隔离设计原则:对方处理速度慢或者不回应，不应该影响其他功能模块或者协议栈。

同步I/O模型很难避免。

2009年，异步Http、异步SOAP、异步SMPP，所有异步非阻塞的协议栈。

Reactor模型。

2014年netty初出茅庐，2015年第二版出版。
