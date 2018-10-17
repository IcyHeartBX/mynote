---
title: shell不能读环境变量问题
tags: 
  - shell
  - 问题
  - 环境变量
  - linux
date: 2018-10-16 19:22:32
categories: shell
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw4acxk758j30i20i2mxc.jpg
---

# nohup: cannot run command `java’: No such file or directory

参考：https://blog.csdn.net/sulong507/article/details/52996657

今天在用`linux crontab` 执行定时任务。在输出的nohup.log文件发现以下错误:

```ruby
  nohup: cannot run command `java’: No such file or directory     
```

`nohup` 执行的命令在当前用户环境没有问题，在`crontab`执行就报以上错误。

开始怀疑是`JAVA_HOME`设置问题，所以使用env进行查看，发现没有问题。

后来怀疑可能是jdk版本问题，因为在1.7上验证没有问题，现在的jdk版本是1.8。

然后把该机器的版本降到1.7发现问题依旧，确定不是版本问题。

因为手工执行没有问题，所以开始怀疑crontab没有正确的加载环境变量。

于是在调用`nohup java -jar之前增加了加载环境变量的操作`，问题解决。

具体实现：/var/spool/crontab/root

`. /etc/profile`



# 自己的问题

我再服务器上部署自动化发布脚本，也遇到这个问题，参考博主的这个在脚本前面加上了

```ruby
source /etc/profile
```

并且执行脚本的时候，使用`source`执行的

```ruby
source reboot.sh
```

问题就解决了

