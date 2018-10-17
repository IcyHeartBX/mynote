---
title: Vim使用笔记
categories:
  - linux
tags:
  - linux
  - vim
  - 学习笔记
date: 2018-10-09 15:49:39
cover_picture: https://ws4.sinaimg.cn/large/006tNbRwly1fw4812szjfj30fa08ljra.jpg
---

![](https://ws4.sinaimg.cn/large/006tNbRwly1fw4812szjfj30fa08ljra.jpg)

# 附录、

## 1、vim E492: Not an editor command: ^M

参考：https://blog.csdn.net/yin_pengpeng/article/details/51674064

原因：

linux的文件换行符为\n，但windows却非要把\r\n作为换行符，所以，vim在解析从windows拷贝到linux的的vimrc时，因为遇到无法解析的\r，所以报错。

解决：设置vim配置文件的文件格式为unix

使用vi打开.vimrc文件，输入命令  :set fileformat=unix



## 2、Mac pip找不到

直接运行

```c
sudo easy_install pip
```

## 3、Vim Vundle Solarized

参考：https://www.cnblogs.com/Alexzzzz/p/6874595.html

- https://blog.csdn.net/hu_fubin/article/details/46573343

## 4、改变光标样式

参考:https://stackoverflow.com/questions/6488683/how-do-i-change-the-vim-cursor-in-insert-normal-mode

```
if exists('$TMUX')
  let &t_SI = "\<Esc>Ptmux;\<Esc>\<Esc>]50;CursorShape=1\x7\<Esc>\\"
  let &t_EI = "\<Esc>Ptmux;\<Esc>\<Esc>]50;CursorShape=0\x7\<Esc>\\"
else
  let &t_SI = "\<Esc>]50;CursorShape=1\x7"
  let &t_EI = "\<Esc>]50;CursorShape=0\x7"
endif
:autocmd InsertEnter * set cul
:autocmd InsertLeave * set nocul
```

## 5、当前行高亮打开

`set cursorline " 高亮当前行`

## 6、backspace按键无法删除字符

参考：https://blog.csdn.net/u010381648/article/details/52244801

```
去掉有关Vi的一致性模式，避免之前版本的Bug，在命令模式下： 
set nocompatible 

设置backspace的工作方式： 
set backspace=indent,eol,start

```

