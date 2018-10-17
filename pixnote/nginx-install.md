---
title: 安装Nginx
tags:
  - nginx
  - linux
  - web
  - server
  - 学习笔记
date: 2018-10-15 13:58:01
categories: web
cover_picture: https://ws4.sinaimg.cn/large/006tNbRwly1fw8w4hzzdcj30h805kglo.jpg
---

# 1、Centos7 安装

由于直接使用

```
yum install nginx -y
```

报找不到

所以，又找了一个文章，成功安装。
参考：https://www.cnblogs.com/tangjiansheng/p/6928722.html

下载对应当前系统版本的nginx包(package)

```
# wget  http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

建立nginx的yum仓库

```
# rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

下载并安装nginx

```
# yum install nginx
```

启动nginx服务

```
systemctl start nginx
```

配置
默认的配置文件在 /etc/nginx 路径下，使用该配置已经可以正确地运行nginx；如需要自定义，修改其下的 nginx.conf 等文件即可。

测试

在浏览器地址栏中输入部署nginx环境的机器的IP，如果一切正常，应该能看到如下字样的内容。