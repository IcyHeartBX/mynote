---
title: 为Linux分配Swap空间
tags:
  - linux
  - git
  - 解决问题
date: 2018-10-15 10:50:04
categories: linux
cover_picture: https://ws1.sinaimg.cn/large/006tNbRwgy1fw8px7aluqj30go09eq35.jpg
---

# 为Linux分配Swap空间

参考：https://blog.csdn.net/jinfeiwang/article/details/50133727

解决一次git的push错误。

因为提交的文件比较大，所以push的时候报错了。

```ruby
Outof memory, mallocfailed” ...
```

其实问题原因是需要一个Swap空间,查看了一下自己的swap空间，太小了

```ruby
free -m
```

所以要重新分配一下。

- 1、添加交换文件（目录路径和大小自己看着办就好了）

  ```ruby
  $ sudo mkdir -p /opt/temp
  $ sudo dd if=/dev/zero of=/opt/temp/swap bs=1024 count=1024000
  1024000+0 records in
  1024000+0 records out
  1048576000 bytes (1.0 GB) copied, 3.74723 s, 280 MB/s
  ```

- 2、创建交换空间

  ```ruby
  $ sudo mkswap /opt/temp/swap
  Setting up swapspace version 1, size = 1023996 KiB
  no label, UUID=99d39e38-4eb2-4b3a-96b8-d623243c657c
  ```

- 3、启动新增加的4G交换空间

  ```ruby
  $ sudo swapon /opt/temp/swap
  swapon: /opt/temp/swap: insecure permissions 0644, 0600 suggested.
  ```

- 4、修改/etc/fstab,使新加的4G交换空间在系统重新启动后自动生效

  ```ruby
  $ sudo echo "/opt/temp/swap swap swap defaults 0 0" >> /etc/fstab
  ```

- 5、看看swap大小

  ```ruby
  $ sudo free -mh
                total        used        free      shared  buff/cache   available
  Mem:           503M        249M         11M         18M        243M        208M
  Swap:          1.1G         43M        1.1G
  ```
