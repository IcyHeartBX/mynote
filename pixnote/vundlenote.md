---
title: Vim Vundle插件管理
categories:
  - linux
tags:
  - linux
  - vim
  - 学习笔记
date: 2018-10-09 15:29:48
cover_picture: https://ws4.sinaimg.cn/large/006tNbRwly1fw4812szjfj30fa08ljra.jpg
---

# 一、Vundle

参考地址：http://blog.csdn.net/zhangpower1993/article/details/52184581

## 1、背景

Vim缺乏默认的插件管理器，所有插件的文件都散布在~/.vim下的几个文件夹中，插件的安装与更新与删除都需要自己手动来，既麻烦费事，又可能出现错误。

### Vundle简介

Vundle 是 Vim bundle 的简称,是一个 Vim 插件管理器. 
**Vundle 允许你做…**

1. 在.vimrc中跟踪和管理插件
2. 安装特定格式的插件(a.k.a. scripts/bundle)
3. 更新特定格式插件
4. 通过插件名称搜索Vim scripts中的插件
5. 清理未使用的插件
6. 可以通过单一按键完成以上操作,详见[interactive mode](https://github.com/VundleVim/Vundle.vim/blob/v0.10.2/doc/vundle.txt#L319-L360)

**Vundle 自动完成**

1. 管理已安装插件的runtime path
2. 安装和更新后,重新生成帮助标签

## 2、**安装vundle**

```shell
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

默认安装在/.vim/bundle/vundle下；

### 配置说明：

**插件有三种类型:** 

1. Github上vim-scripts仓库的插件 

1. Github上非vim-scripts仓库的插件 

1. 不在Github上的插件 

**对于不同的插件，vundle自动管理和下载插件的时候，有不同的地址填写方法，有如下三类：** 

1. 在Github上vim-scripts用户下的仓库,只需要写出repos（仓库）名称 

1. 在Github其他用户下的repos, 需要写出”用户名/repos名” 

1. 不在Github上的插件，需要写出git全路径

## 3、配置vundle插件：

可以在终端通过vim打开~/.vimrc文件，

```
$vim ~/.vimrc
```

也可以直接在目录中打开（快捷键ctrl+H显示隐藏文件）。 
将以下加在**.vimrc文件**中，加入之后保存之后就可以使用vundle了。

**添加的配置信息（样例）** 
**注**：以后安装新插件就直接编辑vimrc，添加plugin就行了，在这里我们添加的plugin只是例子，你可以不安装这些插件，换上自己需要安装的插件。

```shell
set nocompatible              " 去除VI一致性,必须要添加
filetype off                  " 必须要添加

" 设置包括vundle和初始化相关的runtime path
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" 另一种选择, 指定一个vundle安装插件的路径
"call vundle#begin('~/some/path/here')

" 让vundle管理插件版本,必须
Plugin 'VundleVim/Vundle.vim'

" 以下范例用来支持不同格式的插件安装.
" 请将安装插件的命令放在vundle#begin和vundle#end之间.
" Github上的插件
" 格式为 Plugin '用户名/插件仓库名'
Plugin 'tpope/vim-fugitive'
" 来自 http://vim-scripts.org/vim/scripts.html 的插件
" Plugin '插件名称' 实际上是 Plugin 'vim-scripts/插件仓库名' 只是此处的用户名可以省略
Plugin 'L9'
" 由Git支持但不再github上的插件仓库 Plugin 'git clone 后面的地址'
Plugin 'git://git.wincent.com/command-t.git'
" 本地的Git仓库(例如自己的插件) Plugin 'file:///+本地插件仓库绝对路径'
Plugin 'file:///home/gmarik/path/to/plugin'
" 插件在仓库的子目录中.
" 正确指定路径用以设置runtimepath. 以下范例插件在sparkup/vim目录下
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" 安装L9，如果已经安装过这个插件，可利用以下格式避免命名冲突
Plugin 'ascenator/L9', {'name': 'newL9'}

" 你的所有插件需要在下面这行之前
call vundle#end()            " 必须
filetype plugin indent on    " 必须 加载vim自带和插件相应的语法和文件类型相关脚本
" 忽视插件改变缩进,可以使用以下替代:
"filetype plugin on
"
" 常用的命令
" :PluginList       - 列出所有已配置的插件
" :PluginInstall     - 安装插件,追加 `!` 用以更新或使用 :PluginUpdate
" :PluginSearch foo - 搜索 foo ; 追加 `!` 清除本地缓存
" :PluginClean      - 清除未使用插件,需要确认; 追加 `!` 自动批准移除未使用插件
"
" 查阅 :h vundle 获取更多细节和wiki以及FAQ
" 将你自己对非插件片段放在这行之后
```

### 安装需要的插件

1. 将想要安装的插件，按照地址填写方法，将地址填写在**vundle#begin**和**vundle#end**之间就可以
2. 保存之后，有两种方法安装插件。 
   (1) 运行 vim ,再运行 :PluginInstall

```
$vim
:PlugInstall
```

(2) 通过命令行直接安装 vim +PluginInstall +qall

```
vim +PluginInstall +qall
```

安装完成之后，插件就可以使用。

### 移除不需要的插件

1. 编辑.vimrc文件移除的你要移除的插件所对应的plugin那一行。
2. 保存退出当前的vim
3. 重新打开vim，输入命令`BundleClean`。

### 其他常用命令

1. 更新插件`BundleUpdate`
2. 列出所有插件`BundleList`
3. 查找插件`BundleSearch`

### 插件推荐网站

https://vimawesome.com/

# 二、终端配色插件Solarized

参考：https://www.cnblogs.com/Alexzzzz/p/6874595.html

　1.在.vimrc中加入

```shell
Bundle 'Solarized'
```

2.重启gvim，并执行

```shell
:BundleInstall
```

3.将solarized.vim文件放入.vim下的colors文件夹内（如果没有则自己创建）

```shell
mkdir ~/.vim/colors
cd ~/.vim/bundle/Solarized/colors
cp solarized.vim ~/.vim/colors
```

4.更改主题,在.vimrc中加入

```shell
colorscheme solarized
set background=dark
```



# 三、Fugitive.vim

参考：https://blog.csdn.net/panderang/article/details/77913878

[原网页：http://vimcasts.org/episodes/fugitive-vim—a-complement-to-command-line-git](http://vimcasts.org/episodes/fugitive-vim---a-complement-to-command-line-git)

## 1、安装

参考：https://vimawesome.com/plugin/fugitive-vim

在`.vimrc`文件中加入

```shell
Plugin 'tpope/vim-fugitive'
```

在vim中运行

```shell
:BundleInstall
```

## 2、使用一

使用 :Git 命令你可以从 VIM 命令行中运行任何的 git 命令。使用该命令它会切换的 Shell 去显示命令的输出，就像是 git log 命令一样。但是对于一些没有输出的命令，:Git 依然也会相当公平切换到 Shell 。需要你按 Enter 键返回。

　　在 Vim 命令行中% 有着特殊的含义，它会被解释成当前文件的全路径。因此该符号可以用在任何需要当前文件路径的 git 命令中。但是 fugitive 也提供了一些方便的命令，如下表所示：

| git             | fugitive | action                           |
| --------------- | -------- | -------------------------------- |
| :Git add %      | :Gwrite  | 将当前文件存入暂存区             |
| :Git checkout % | :Gread   | 恢复当前文件到最新版本           |
| :Git rm %       | :Gremove | 删除当前文件和相应的vim buffer   |
| :Git mv %       | :Gmove   | 重命名当前文件和相应的vim buffer |

　　:Gcommit 命令将会打开一个水平分隔的窗口填写提交信息并使用 :wq 命令完成。该命令提交的好处是在填写 Commit 信息时可以使用 vim 的补全特性。

　　:Gblame 命令将会打开一个垂直分隔的窗口，其中包含有对每一行的注释信息。这些注释信息包括最新的 commit 号，作者，时间。该垂直分隔的窗口和文件窗口是滚动绑定的，因此一个文件向下滚动另一个窗口也会跟着滚动。

![ç¤ºä¾](http://oj8tpbqx0.bkt.clouddn.com/Csdn_Blog/BlogPics/vim-004-1.gif)

更多内容可见 fugitive 帮助文档，在vim命令模式输入以下命令。

- :help cmdline-special
- :help :_%
- :help ‘complete’
- :help :Git
- :help :Gwrite
- :help :Gread
- :help :Gremove
- :help :Gmove
- :help :Gcommit
- :help :Gblame

 ## 3、使用二

参考：https://blog.csdn.net/panderang/article/details/77913889

[原网页：http://vimcasts.org/episodes/fugitive-vim-working-with-the-git-index/](http://vimcasts.org/episodes/fugitive-vim-working-with-the-git-index/)

### :Gstatus窗口

　　:Gstatus命令会打开一个窗口显示当前 git 仓库的状态，其内容和 git status 命令所展示的内容相一致。但是 :Gstatus 所打开的窗口将会提供更多的交互操作。相关操作命令如下：

| 命令 | 作用                            |
| ---- | ------------------------------- |
| -    | 添加/删除文件                   |
| \    | 向下分割一个窗口打开当前文件    |
| P    | 为当前文件运行 “git add -patch” |
| C    | 调用 :Gcommit                   |

　　使用实例如下：![2.1](http://oj8tpbqx0.bkt.clouddn.com/Csdn_Blog/BlogPics/vim-005-1.gif)

### 使用 git index

　　git index 就是最近一次提交的文件版本，也是下一次 commit 提交的地方。详细介绍可见 [the git index](http://shafiulazam.com/gitbook/1_the_git_index.html)。在 VIM 命令中键入 :Gedit :path/to/file 就可以打开任意以文件 index 版本。键入以下命令可以打开当前文件的index版本（index版本，多用于下面即将介绍的 Gdiff 文件比较）。

- :Gedit
- :Gedit :0
- :Gedit :%

### :Gdiff 使用

　　在 git 仓库目录中打开用vim打开一文件，并在 vim 命令行中键入没有任何参数的 :Gvdiff 命令。fugitive 将会展现出一个类似于 vimdiff 的表现形式来比较当前文件和当前文件的 index 版本。Gvdiff 将会以垂直分屏的方式打开另一个窗口，index版本的文件位于左边，当前工作文件位于右边。如下图所示：

![2.2](http://oj8tpbqx0.bkt.clouddn.com/Csdn_Blog/BlogPics/vim-005-2.gif)

### :Gread / :Gwrite 与 :diffput / :diffget

　　:Gread / :Gwrite 命令都可以添加（git add）或重新检出（git checkout）一个文件，根据它们所运行的窗口不同而不同。对 Index 文件进行写入（:w）操作就相当于就行 git add 操作。总结如下表所示：

| 命令    | 当前窗口     | 效果          |
| ------- | ------------ | ------------- |
| :Gwrite | Working file | stage file    |
| :Gread  | Working file | checkout file |
| :Gwrite | Index file   | checkout file |
| :Gread  | Index file   | stage file    |

![2.3](http://oj8tpbqx0.bkt.clouddn.com/Csdn_Blog/BlogPics/vim-005-3.gif)

　　:diffget / :diffput 与 :Gread / :Gwrite 类似。 :Gread / :Gwrite 是对整个文件进行操作。即用 working file 覆盖 index File 或者是用 index file 覆盖 working file。 而:diffget / :diffput可以将Working file 中部分更改提交到 index file 再对 index file 进行 :w 操作就可以将部分刚给提交到暂存区（stage），其操作类似于 git add –patch。实例如下图所示：

![2.4](http://oj8tpbqx0.bkt.clouddn.com/Csdn_Blog/BlogPics/vim-005-4.gif)

