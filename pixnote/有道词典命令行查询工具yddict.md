---
title: 有道词典命令行查询工具yddict
categories:
  - linux
tags:
  - shell
  - 工具
  - terminal
  - 词典
cover_picture: https://ws1.sinaimg.cn/large/006tNbRwgy1fw44d0ls8qj319k0iytc5.jpg
date: 2018-10-09 13:29:57
---

# [ 有道词典命令行查询工具（Mac/Ubuntu）](https://www.cnblogs.com/EasonJim/p/8476850.html)

参考：https://www.cnblogs.com/EasonJim/p/8476850.html

说明：此工具是基于node.js的，所以必须安装npm。

官网：<https://github.com/kenshinji/yddict>

安装：

Mac：

```shell
# 安装npm
brew install npm
# 安装有道
npm install yddict -g
```

Ubuntu：

```shell
# 安装npm
wget https://nodejs.org/dist/v6.9.5/node-v6.9.5-linux-x64.tar.xz
tar -xvf node-v6.9.5-linux-x64.tar.xz
sudo mv node-v6.9.5-linux-x64 /usr/local/node
sudo ln -s /usr/local/node/bin/node /usr/local/bin/node
sudo ln -s /usr/local/node/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm
# 安装有道
npm install yddict -g
# 配置
ln -s  /usr/local/node/lib/node_modules/yddict/index.js /usr/local/bin/yd
```

使用：

```shell
yd 测试
```

![屏幕快照 2018-10-11 上午11.23.21](https://ws1.sinaimg.cn/large/006tNbRwgy1fw44d0ls8qj319k0iytc5.jpg)