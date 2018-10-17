---
title: 树莓派学习笔记
categories:
  - 树莓派
tags:
  - 树莓派
  - 学习笔记
date: 2018-10-08 10:24:14
cover_picture: https://ws1.sinaimg.cn/large/006tNbRwly1fw483bsoo2j307408x0sv.jpg
---

# 一、树莓派3B+ 安装系统

参考：https://blog.csdn.net/kxwinxp/article/details/78370913

> 对于树莓派3B+ 系统安装方法有很多，我就介绍比较普通的一种。适合小白操作！
> 安装概要步骤： 官网下载系统-》刷入TF卡-》设置开启显示器和SSH-》通电-》进入系统

## 1、下载镜像

1. 进入官方网站下载系统镜像。
   下载页面：https://www.raspberrypi.org/downloads/
   x如果感觉下载速度慢，可以将下载链接放到迅雷里面下，基本可以做到满速下载！
   如果你对我后续的博文有兴趣，建议和我下载相同版本： 
   stretch版 (基于Debian 9): 2017-09-07-raspbian-stretch.zip 或 2017-09-07-raspbian-stretch-lite.zip 
   更老版 jessie (基于Debian 8)：2017-06-21-raspbian-jessie.zip 和 2017-06-21-raspbian-jessie-lite.zip
   下载完成后，是一个压缩包，大概1.76G，我们将其解压，得到2017-09-07-raspbian-stretch.img格式文件，大概4.92G。 
   如果你下载的是轻量版，解压后大概就是1.7G，可以装到4G到TF卡上。（在这里建议大家用32G及以上容量的TF卡，因为内存越大，传输速度也是更快的）。

## 2、Windows 安装

2.1）首先将准备好的TF卡连接读卡器，插入电脑
2.2）下载一个格式化SD卡的工具，格式化SD卡
下载网址：https://www.sdcard.org/downloads/formatter_4/eula_windows/ （点击Aceept开始下载）

2.3）下载Win32 DiskImager，这是一个把镜像写入SD卡的工具
下载网址：http://sourceforge.net/projects/win32diskimager/

这一步首先选择你的raspberry.img系统镜像包，然后选择你的TF卡，点击Write就会开始工作了，大概3～4分钟左右。

## 3、Mac安装

2.1）首先将准备好的TF卡连接读卡器，插入电脑
2.2）打开终端（Terminal）,查看当前已挂载的卷：
[kxwinxp@MacBook]$ df -h

```
Filesystem                Size   Used  Avail Capacity iused      ifree %iused  Mounted on
/dev/disk1               112Gi  103Gi  8.3Gi    93% 2258270 4292709009    0%   /
devfs                    337Ki  337Ki    0Bi   100%    1166          0  100%   /dev
map -hosts                 0Bi    0Bi    0Bi   100%       0          0  100%   /net
map auto_home              0Bi    0Bi    0Bi   100%       0          0  100%   /home
/dev/disk2s4             300Gi  253Gi   47Gi    85% 2072064     385447   84%   /Volumes/Data
/dev/disk2s3             165Gi   40Gi  125Gi    25%      44  131301272    0%   /Volumes/Untitled
/Applications/iTerm.app  112Gi  104Gi  7.4Gi    94% 2257800 4292709479    0%   /private/var/folders/qt/7c2l6sjs077bfbbf82d8m5c40000gn/T/AppTranslocation/BDB3F792-EE55-4D31-A21D-7BB40DC6619D
/dev/disk3s4              14Gi  2.6Mi   14Gi     1%      82     473546    0%   /Volumes/SDCard1
```

对比Size和Name可以找到SD卡的分区在系统里对应的设备文件（这里是/dev/disk3s1），如果你有多个分区，可能还会有disk3s2之类的。

2.3）使用diskutil unmount将这些分区卸载：
`[kxwinxp@MacBook]$ diskutil unmount /dev/disk3s1`
Volume 未命名 on disk3s1 unmounted
2.4）先对下载的zip压缩包进行解压，然后使用dd命令将系统镜像写入，需要特别特别注意disk后的数字，不能搞错！
（说明：/dev/disk3s1是分区，/dev/disk3是块设备，/dev/rdisk3是原始字符设备）

```shell
[kxwinxp@MacBook] unzip 2017-09-07-raspbian-stretch.zip
[kxwinxp@MacBook] sudo dd bs=16m if=2017-09-07-raspbian-stretch.img of=/dev/rdisk3
```

我使用这个命令刷入：

```shell
sudo pv -petr 2018-06-27-raspbian-stretch.img |sudo dd of=/dev/rdisk3 bs=10m
```

经过几分钟的等待，出现下面的提示，说明TF卡刷好了：

1172+1 records in
1172+1 records out
4916019200 bytes transferred in 127.253638 secs (9691442 bytes/sec)

好了，系统已经刷入TF卡了。

# 二、中文输入法

参考：http://shumeipai.nxez.com/2015/03/11/raspberry-pi-to-install-chinese-input-method-fcitx-and-google-pinyin-ime.html

```shell
sudo apt-get install fcitx fcitx-googlepinyin fcitx-module-cloudpinyin fcitx-sunpinyin
```

安装完，重启。

# 三、Wifi设置

参考：https://blog.csdn.net/bailyzheng/article/details/33336709



# 四、wifi掉线问题

参考：https://www.douban.com/note/683337698/

```shell
网上搜了很多解决方案，基本分两种：

1. 后台脚本，发现断线后重启网卡或者后台运行ping命令来保持连接；

2. 关闭无线设备的电源管理。

关闭无线设备电源管理的方法基本以下两种：

1）运行命令：

  sudo iwconfig wlan0 power off 

或者：

  sudo iw dev wlan0 set power_save off 

2）编辑或创建配置文件：

  sudo nano /etc/modprobe.d/8192cu.conf 

写入内容：

      # Disable power saving
     options 8192cu rtw_power_mgnt=0 rtw_enusbss=1 rtw_ips_mode=1 

试下来，关闭电源管理的方法没起作用。

只能采用第一种方案。开机运行命令：

  ping 192.168.1.1 > /dev/null & 
```

另一种说法：

http://shumeipai.nxez.com/2017/01/25/raspberry-pi-wifi-broken-automatically-reconnect.html

```
这个断开大概是因为树莓派能耗机制的问题，总是重连终归不是办法，推荐如下解决方式：
创建并编辑文件/etc/modprobe.d/8192cu.conf
sudo nano /etc/modprobe.d/8192cu.conf
并且粘贴下列内容
# Disable power saving
options 8192cu rtw_power_mgnt=0 rtw_enusbss=1 rtw_ips_mode=1
然后使用sudo reboot进行重启。
```

## 1、我的wifi掉线解决方案

参考：https://www.raspberrypi.org/forums/viewtopic.php?t=188891

修改

```shell
vim /etc/wpa_supplicant/wpa_supplicant.conf
```

中

```SHELL
country=CN
```

或者

使用菜单选项 - >首选项 - > Raspberry PI配置 - >本地化 - >设置WiFi国家/地区。

即可，我直接选择了US,导致有的路由器连接会频繁掉线。



# 五、树莓派源

https://blog.csdn.net/la9998372/article/details/77886806/



# 六、安装Nginx

https://blog.csdn.net/freddy_yu/article/details/76165129



