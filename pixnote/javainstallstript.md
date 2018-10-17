---
title: java linux安装脚本
tags:
  - java
  - linux
  - shell
  - 学习笔记
date: 2018-10-14 14:11:38
categories: shell
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw4acxk758j30i20i2mxc.jpg
---

# java在linux中的安装脚本

这里写一个脚本，下载了Oracle的java 11的压缩包。方便安装。

安装脚本保存在：https://github.com/IcyHeartBX/install_shell

`jdk_install.sh`

```ruby
#!/bin/bash
# jdk install stript
echo "=== Install jdk start ==="
cd res
sudo tar -zxvf jdk-11_linux-x64_bin.tar.gz -C /usr/local/
echo "Install jdk to /usr/local path!"
sudo echo '''
JAVA_HOME=/usr/local/jdk-11
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.
export JAVA_HOME CLASSPATH
        ''' >> /etc/profile
sh /etc/profile
echo "=== Install jdk success! ==="
```

