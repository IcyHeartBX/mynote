---
title: 从远程服务器下载文件脚本
tags:
  - shell
  - linux
date: 2018-10-14 14:37:57
categories: shell
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw4acxk758j30i20i2mxc.jpg
---

# 从远程服务区下载文件脚本

## 1、脚本创建

这里写一个从远程服务器下载文件脚本。

故事的原因是公司从公司的服务器上下载`github`上的东西太慢了。但是从远程`VPS服务器`下载特别快。从服务器下载到公司也特别快。

所以写一个脚本，下载东西的路径变成 `VPS服务器下载文件` **-->** `公司网络下载VPS服务器上的文件`。

脚本：`download_from_vps.sh`

```ruby
#!/bin/bash
# download from vps server
DOMAIN=
PORT=
DOWNLOAD_URL=""
if [ -z "$1" ];then
    echo "Please wirte download url"
    read DOWNLOAD_URL
else
    DOWNLOAD_URL="$1"
fi
echo "download : $DOWNLOAD_URL"

# download from server
ssh -p $PORT root@$DOMAIN "cd /root/Downloads/download_service_d; wget $DOWNLOAD_URL"
# copy file to local
scp -P $PORT -r root@$DOMAIN:/root/Downloads/download_service_d/* .
# delete all
ssh -p $PORT root@$DOMAIN "rm -rf /root/Downloads/download_service_d/*"
```

## 2、脚本使用

在脚本中`DOMAIN`变量赋予服务器IP,`PORT`赋予`ssh`协议端口号

如：下载`google的protobuf`

```ruby
sh download_from_vps.sh https://github.com/protocolbuffers/protobuf/archive/master.zip
```



