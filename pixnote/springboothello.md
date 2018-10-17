---
title: (学习)SpringBoot入门及搭建
tags:
  - springboot
  - web
  - java
date: 2018-10-12 11:12:17
categories: web
cover_picture: https://ws4.sinaimg.cn/large/006tNbRwly1fw59run3rrj30d607imx6.jpg
---

# SpringBoot入门及搭建

参考：（摘抄自）https://juejin.im/post/5b69949bf265da0f4f1663af

## 1、SpringBoot简介

和书上理解的不同，我认为Springboot是一个优秀的快速搭建框架，他通过maven继承方式添加依赖来整合很多第三方工具，可以避免各种麻烦的配置，有各种内嵌容器简化Web项目，还能避免依赖的干扰，它内置tomcat，jetty容器，使用的是java app运行程序，而不是传统的用把war放在tomcat等容器中运行。

## 2、和JFinal的区别

JFinal是国人出品的一个web + orm 框架 ，[JFinal](https://link.juejin.im?target=http%3A%2F%2Fwww.jfinal.com%2F)，优点是开发迅速、代码量少、学习简单、功能强大、轻量级、易扩展。核心就是极致简洁。他没有商业机构的支持，所以宣传不到位，少有人知。

Springboot相比与JFinal最大的优点就是支持的功能非常多，可以非常方便的将spring的各种框架如springframework , spring-mvc, spring-security, spring-data-jpa, spring-cache等等集成起来进行自动化配置 ，而且生态 比较好，很多产品都对Springboot做出一定支持。

## 3、与Springcloud的区别

可以这么理解，Springboot里面包含了Springcloud，Springcloud只是Springboot里面的一个组件而已。

Springcloud提供了相当完整的微服务架构。而微服务架构，本质来说就是分布式架构，意味着你要将原来是一个整体的项目拆分成一个个的小型项目，然后利用某种机制将其联合起来，例如服务治理、通信框架等基础设施。

## 4、SpringBoot和SpringMVC区别

SpringBoot的Web组件，默认集成的是SpringMVC框架。

## 5、快速使用

要往下看的话，注意了👇

- Springboot 2.x 要求 JDK 1.8 环境及以上版本。另外，Springboot  2.x 只兼容 Spring Framework 5.0 及以上版本。
- 为 Springboot 2.x 提供了相关依赖构建工具是 Maven，版本需要 3.2 及以上版本。使用 Gradle 则需要 1.12 及以上版本。
- 建议用IntelliJ IDEA IntelliJ IDEA （简称 IDEA）

## 6、建立项目

IDEA创建Springboot的方法



# 附录学习资料参考

- https://blog.csdn.net/qq_31655965/article/details/72828095
- 