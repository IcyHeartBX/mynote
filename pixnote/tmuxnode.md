---
title: Tmux终端复用工具
tags:
  - linux
  - terminal
  - 学习笔记
date: 2018-10-16 13:29:08
categories: linux
cover_picture: https://ws1.sinaimg.cn/large/006tNbRwgy1fw8px7aluqj30go09eq35.jpg
---

# 一、tmux复用详解

## 1、tmux是什么

摘抄参考自：https://www.cnblogs.com/wangqiguo/p/8905081.html

我们在linux服务器上的工作一般都是通过一个远程的终端连接软件连接到远端系统进行操作，例如使用xshell或者SecureCRT工具通过ssh进行远程连接。在使用过程中，如果要做比较耗时的操作，例如有时候进行编译，或者下载大文件需要比较长的时间，一般情况下是下班之后直接运行希望第二天早上过来运行完成，这样就不用耽误工作时间。但是网络有时候不稳定，可能在半夜会出现连接断掉的情况，一旦连接断掉，我们所执行的程序也就中断，我们当然可以写一个脚本后台运行，但是还是不方便。那么有没有一种工具可以解决这样的问题呢。这就是我们这里要提到的tmux了。其实类似tmux的工具还有很多。例如gnu screen等。tmux刚好可以解决我们描述的问题，当我们在tmux中工作的时候，即使关掉SecureCRT的连接窗口，再次连接，进入tmux的会话我们之前的工作仍然在继续。

tmux是一个linux下面的工具，在使用之前需要安装，就像安装linux下的其他工具一样方便。首先我们通过SecureCRT连接登入远程的linux机器，我们将此时的环境称为终端环境。如果这个机器上并没有安装tmux，我们需要安装。例如在CentOs上是yum install tmux，完成之后我们就可以使用tmux命令了。tmux中有3种概念，会话，窗口(window)，窗格(pane)。会话有点像是tmux的服务，在后端运行，我们可以通过tmux命令创建这种服务，并且可以通过tmux命令查看，附加到后端运行的会话中。一个会话可以包含多个窗口，一个窗口可以被分割成多个窗格(pane)。首先我们来看一下tmux的会话。

## 2、tmux的安装

tmux的安装非常简单

debain系：

```ruby
sudo apt-get install tmux
```

redhat系：

```ruby
sudo yum install tmux
```

MacOS

```ruby
sudo brew install tmux
```

## 3、tmux的会话

### 3.1、新建会话session

使用 `tmux new -s` 命令新建一个会话 -s (其实是session的头字母)。后面指定会话名即可。运行之后会从shell的终端环境进入到会话环境中，并停留在刚才新建的会话中。例如：

![o_session](https://ws4.sinaimg.cn/large/006tNbRwly1fwa0d81ygej30me0cldho.jpg)

可以看到进入session之后的显示，在下面有一条绿色的状态指示栏，左边显示的是当前会话的名字，紧接着是会话中的窗口(window)序号以及窗口名字。关于窗口的概念我们后面再说，窗口名字后面有一个星号*表示是我们操作的当前窗口，一个会话中可以有多个窗口。当进入一个会话之后，会自动创建一个窗口。如上图所示，上面的环境在本章中称为会话环境。这样我们就已经开始了tmux的使用，如果此时关闭掉SecureCRT软件，下次在进入，该会话仍然在运行工。也就是说我们在刚刚进入的会话环境中使用wget下载一个超大的文件，或者是编译一个非常耗时的项目，我们关闭掉该SecureCRT的连接，下次再进入，这个会话依然存在，会话里面运行的编译命令或者wget下载命令仍然在运行，并不会因为关闭SecureCRT而终止，这正是我们需要的功能。不受SecureCRT网络连接的影响。甚至我们可以关掉整个SecureCRT程序。

### 3.2、退出会话

`ctrl+b d`退出会话，回到shell的终端环境

我们刚才是通过 tmux new -s 命令创建一个tmux会话并进入该会话的，如果要退出这个会话环境回到终端环境(会话里面的程序不会退出在后台保持继续运行)。应该如何操作呢，例如上图，当前我们在tmux的会话环境中，使用一个快捷键 `ctrl+b d` (按ctrl+b 之后再按一个字母d即可，字母d是detach的缩写)。操作之后的结果如下：

![o_detatch](https://ws4.sinaimg.cn/large/006tNbRwly1fwa0h8clcdj30h90awgmx.jpg)

可以看到绿色的状态栏消失了，而顶部出现一个[detached]，表示已经脱离tmux会话，现在已经不在tmux的会话环境中回到shell终端环境中了。

这里有必要说一下在tmux会话环境中，我们经常会用到tmux的组合键，一般的组合键中都会加一个前缀也就是 ctrl+b 另外，在后面的描述中，我们说的终端环境是指使用SecureCRT进入远程linux之后但是没有进入tmux的会话环境的状态。

通过上面的操作 ctrl+b d 之后，回到终端环境，实际上现在tmux的会话还在后台运行，如何查看呢。

### 3.3、查看会话列表

`tmux ls `终端环境查看会话列表

在终端环境中，我们可以通过tmux ls 命令来查看后台运行中的tmux的会话列表，例如：

```ruby
$ tmux ls
0: 2 windows (created Tue Oct 16 10:20:24 2018) [100x27]
1: 2 windows (created Tue Oct 16 10:37:41 2018) [105x28]
10: 1 windows (created Tue Oct 16 12:14:33 2018) [172x41]
11: 1 windows (created Tue Oct 16 12:15:05 2018) [150x19]
12: 2 windows (created Tue Oct 16 12:16:41 2018) [150x43]
13: 1 windows (created Tue Oct 16 12:18:25 2018) [150x35]
15: 1 windows (created Tue Oct 16 12:25:33 2018) [171x41]
16: 1 windows (created Tue Oct 16 13:44:11 2018) [150x44] (attached)
2: 1 windows (created Tue Oct 16 11:12:51 2018) [133x36]
3: 2 windows (created Tue Oct 16 11:23:39 2018) [172x52]
4: 1 windows (created Tue Oct 16 12:00:17 2018) [142x21]
5: 2 windows (created Tue Oct 16 12:01:30 2018) [172x52]
6: 1 windows (created Tue Oct 16 12:03:48 2018) [137x37]
7: 1 windows (created Tue Oct 16 12:06:47 2018) [80x24]
8: 2 windows (created Tue Oct 16 12:11:07 2018) [172x41]
9: 1 windows (created Tue Oct 16 12:11:41 2018) [150x43]
```

其中左边的1 ~ 16表示该session的名字，中间2 windows说明该session会话中有2个window，右边表示该会话创建的时间。如果该机器中有多个tmux会话在后台运行，那么这里会列出多行。因为tmux会话在后台运行，我们猜测实际上肯定是有tmux的进程在后台运行来维持这些会话。我们可以ps看一下：

```ruby
$ ps -ef | grep tmux
  501  2277     1   0 10:20AM ??         0:02.03 tmux
  501  6465  5388   0  1:44PM ttys026    0:00.01 tmux
  501  6515  6466   0  1:47PM ttys033    0:00.00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn tmux
```

可以看到我们之前创建会话的命令还在后端运行。

### 3.4、查看会话列表

`ctrl+b s` 会话环境查看会话列表

上面的命令中我们已经退出了tmux的会话环境，在终端环境中通过tmux ls 来列出当前linux机器后台运行的tmux会话列表。那么假设我们当前环境已经在tmux的会话环境中，我们如何得到当前tmux的会话列表呢，如果每次都要退出当前会话，先回到shell终端环境再运行tmux ls 来查看就很不方便，那么在tmux的会话环境中，我们可以通过 ctrl+b s 来获取当前linux机器上tmux所有的后台会话列表，例如操作之后显示如下：

![o_sessionls](/Users/tangpengxiang/Desktop/o_sessionls.png)

此时可以通过方向键选择会话并回车，在会话间进行切换。

### 3.5、从终端环境进入会话

`tmux a -t session1` 从终端环境进入会话

如果在终端环境中运行 `tmux ls` 查看有tmux会话正在后台运行，如何进入到该正在后台中运行的会话呢，通过运行 `tmux a -t session1` 即可进入到该已存在的会话 session1 中。其中a字母是attach的头字母，表示附加， -t 指定要进入已存在的会话名，如果不存在则会报告 session not found 错误。

### 3.6、销毁会话

`tmux kill-session -t session1` 销毁会话

我们可以在终端环境和会话环境中销毁会话，例如在终端环境中运行` tmux kill-session -t session1` 结束名字为session1的tmux会话。

在会话环境中运行 `ctrl+b : `(注意按组合键之后再按一个冒号键)，状态栏变成黄色之后提示我们可以在会话环境中输入命令，此时输入 kill-session -t session1 回车即可。其中session1是要销毁的会话名。

会话销毁之后，在终端环境中运行tmux ls 或者在会话环境中运行 ctrl+b s 则被销毁的会话不会再出现在会话列表中。

### 3.7、重命名会话

`tmux rename -t old_session_name  new_session_name`  重命名会话

我们可以在终端环境中将会话重命名，如上面的命令，重命名之后通过 tmux ls 命令在终端环境中看到的列表中会显示会话的新名称。

### 3.8、重命名会话 (在会话环境中)

` ctrl + b $` 重命名会话 (在会话环境中)

在会话环境中，我们可以通过前缀命令加上 $ 的组合来重命名当前打开的会话的名字



## 4、tmux的window

一个tmux的会话中可以有多个窗口(window)，每个窗口又可以分割成多个pane(窗格)。我们工作的最小单位其实是窗格。默认情况下在一个window中，只有一个大窗格，占满整个窗口区域。我们在这个区域工作。

本节我们讲解一下tmux窗口的相关操作，后面我们再说一下关于窗格(pane)的相关知识。首先在新创建的一个会话里面是会默认创建一个窗口的。正如我们上面提到过的图一样，如下所示：

![o_session](https://ws2.sinaimg.cn/large/006tNbRwgy1fwa13c1z8uj30me0cldho.jpg)

新创建的会话中会默认创建一个窗口，该窗口名字一般是登陆终端的用户名@主机名，我们可以通过 `crtl+b , `(组合键之后按一个逗号)来修改当前窗口的名字，如上图所示的窗口名字myserver1就是修改之后的名字。该名字后面有一个*号，表示该窗口是活动窗口(键盘输入会输入到该窗口中)

### 4.1、创建window

可以在当前会话窗口中创建多个窗口，例如` ctrl+b c `创建之后会多出一个窗口如下图所示：

![o_create_window](/Users/tangpengxiang/Desktop/o_create_window.png)

默认情况下创建出来的窗口由窗口序号+窗口名字组成，窗口名字可以由上面提到的方法修改，可以看到新创建的窗口后面有*号，表示是当前窗口。

### 4.2、切换window

在同一个会话的多个窗口之间可以通过如下快捷键进行切换：

- `ctrl+b p` (previous的首字母) 切换到上一个window。

- `ctrl+b n` (next的首字母) 切换到下一个window。

- `ctrl+b 0` 切换到0号window，依次类推，可换成任意窗口序号

- `ctrl+b w` (windows的首字母) 列出当前session所有window，通过上、下键切换窗口

- `ctrl+b l` (字母L的小写)相邻的window切换

- `ctrl+b &` 关闭window

- `ctrl+b &` 关闭当前window，会给出提示是否关闭当前窗口，按下y确认即可。

## 5、tmux的pane

tmux的一个窗口可以被分成多个pane(窗格)，可以做出分屏的效果。

### 5.1、垂直分屏

`ctrl+b %` 垂直分屏(组合键之后按一个百分号)，用一条垂线把当前窗口分成左右两屏。

![o_pane_v](https://ws2.sinaimg.cn/large/006tNbRwgy1fwa1x7irhyj30jc0fajrv.jpg)

### 5.2、水平分屏

`ctrl+b "` 水平分屏(组合键之后按一个双引号)，用一条水平线把当前窗口分成上下两屏。

![o_pane_h](https://ws2.sinaimg.cn/large/006tNbRwgy1fwa21lchx1j30j80f9mxj.jpg)

分屏之后光标停留在哪个pane上，表示该pane是活动的，另外一般情况下当前pane会被绿色的线条围起来。一般分屏之后当前窗口名字会重置为默认窗口名字。通过多次分屏操作，我们可以得到各种样子的分屏效果，例如下图显示的是一次垂直分屏之后，在右边pane中再次水平分屏的效果：

![o_pane_multi](https://ws2.sinaimg.cn/large/006tNbRwly1fwa25rjzr3j30jb0f8dgc.jpg)

可以看到右下角的分屏是绿色框，说明是当前活动pane

### 5.3、切换pane

- `ctrl+b o` 依次切换当前窗口下的各个pane。

- `ctrl+b Up|Down|Left|Right` 根据按箭方向选择切换到某个pane。

- `ctrl+b Space` (空格键) 对当前窗口下的所有pane重新排列布局，每按一次，换一种样式。

- `ctrl+b z` 最大化当前pane。再按一次后恢复。

### 5.4、关闭pane

`ctrl+b x` 关闭当前使用中的pane，操作之后会给出是否关闭的提示，按y确认即关闭。

## 6、总结

tmux中的最重要的三个概念会话，窗口，pane的使用方法已经介绍完毕，其实这是我们操作tmux的最常用功能，如果掌握好，足以应付大多数工作。另外tmux还有一些高级用法，例如可以个性化的配置其组合键(官方默认的ctrl+b组合键按起来不太方便可以修改，UI设置，鼠标支持，复制粘贴等)，但是我觉得这些高级功能基本不太用的到。如有需要大家可以自行查阅相关资料。

# 二、配置文件

在宿主目录下创建一个`~/.tmux.conf`的文件作为配置文件

```shell
vim ~/.tmux.conf
```

加载配置文件，在tmux中输入`C-B :`进入命令模式

```ruby
source-file ~/.tmux.conf
```

加载配置文件，注意，这里不是在shell中`source ~/.tmux.conf`这是加载不了的

## 1、加载配置文件快捷键

将`C-B r`作为加载配置文件快捷键

```ruby
#将r 设置为加载配置文件，并显示"reloaded!"信息
bind r source-file ~/.tmux.conf \; display "Reloaded!"
```

## 2、配置窗口边界颜色

```ruby
# 分割窗口边界的颜色
set-option -g pane-active-border-fg '#00FF00'
set-option -g pane-border-fg '#FF0000'
```

刚开始的时候，我分割窗口没有分界线，找了很多原因，原来是我的MacOS的item2的字体原因。

如果有碰到不出分界线的情况，试着修改终端的字体试一试。





# 附录、

## 1、一些学习参考

- https://www.cnblogs.com/kevingrace/p/6496899.html
- https://minimul.com/increased-developer-productivity-with-tmux-part-1.html
- https://www.cnblogs.com/bamanzi/p/tmux-mouse-tips.html
- https://blog.csdn.net/ZCF1002797280/article/details/51859524
- https://blog.csdn.net/chenqiuge1984/article/details/80132042
- 