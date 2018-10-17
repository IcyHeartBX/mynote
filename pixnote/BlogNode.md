---
title: 博客搭建过程
date: 2018-09-29 15:55:04
tags: 
- 学习笔记
- blog
- hexo
categories: blog
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw47vwd01yj30fa03mmx2.jpg
---

![u=888502118,2306802280&fm=26&gp=0](https://ws2.sinaimg.cn/large/006tNbRwly1fw47vwd01yj30fa03mmx2.jpg)

# 一、搭建

想做一个博客系统，来整理自己的知识点

js可以做前端，也可以做后端nodejs也很强大。所以找下nodejs开源项目。

找到两个，如下：

- https://github.com/eshengsky/iBlog2   一个个人项目，界面清爽
- https://www.w3cschool.cn/liblog/liblog-rcw32288.html 比较大的开源项目

## 1、参考iBlog2搭建

这个搭建成功，无法修改其中的内容，无法创建帖子。需要研究后修改才可以。



## 2、Hexo博客系统搭建

从网上找了下，这个比较主流，所以试着搭建一下。

### 2.1、VPS

参考：https://www.jianshu.com/p/605c3d32cab9

#### 前期准备

为什么选择 Hexo 呢？ 我的理由很简单

- 喜欢Hexo的主题，不少都适合中文
- 配置简单，一键发布
- 创作者是台湾人，支持一下国人的开发成果！

关于如何搭建 Hexo 的环境，我在这里就不罗嗦了， [Hexo官方文档](https://link.jianshu.com?t=https://hexo.io/zh-cn/docs)已经讲解的非常相信了，因为 Hexo 是国人发明的所以，官方文档本身就有汉语版本。 推荐使用 [NexT.Mist](https://link.jianshu.com?t=http://theme-next.iissnan.com/) 作为 Hexo 的主题，个人感觉很简洁很美观。

#### VPS 上安装 Nginx 服务

SSH 连接 VPS 后，添加 CenOS 7 的 epel 软件包

```shell
$ yum install epel-release
```

安装Nginx

```shell
$ yum install nginx
```

在自己的VPS Centos7上安装，找不到软件包。

由于直接使用

```
yum install nginx -y
```

报找不到

所以，又找了一个文章，成功安装。 参考：<https://www.cnblogs.com/tangjiansheng/p/6928722.html>

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

配置 默认的配置文件在 /etc/nginx 路径下，使用该配置已经可以正确地运行nginx；如需要自定义，修改其下的 nginx.conf 等文件即可。

测试

在浏览器地址栏中输入部署nginx环境的机器的IP，如果一切正常，应该能看到如下字样的内容。

启动 Nginx

```
$ systemctl start nginx.service
```

使用 firewalld 给防火墙添加规则允许 HTTP 以及 HTTPS (不知道 firewalld 的，请查看[上篇文章](https://link.jianshu.com?t=2015/04/12/build-your-own-vps/))

```
$ firewall-cmd --permanent --zone=public --add-service=http
$ firewall-cmd --permanent --zone=public --add-service=https
$ firewall-cmd --reload
```

设置 Nginx 自动跟随系统启动

```
$ systemctl enable nginx.service
```

现在可以在浏览器中输入 VPS 的 ip 检查看 Nginx! 是否启动了。
 如果出现 "Welcome to Nginx.." 的字样，恭喜！代表你的 Nginx 成功安装并启动

#### 配置Nginx工作目录

修改`/etc/nginx/conf.d/default.conf`

```
location / {
        root   /xxx/dev/blog/hexo;
        index  index.html index.htm;
    }
```

修改完成，重启nginx

`service nginx restart`

如果报403错误，修改nginx用户名

`vim /etc/nginx/nginx.conf`

```java
user  xxx; // 改为工作目录权限用户
```



### 2.2、配置Hexo

参考：https://www.jianshu.com/p/8c55f103e8b4

#### 什么是Hexo？

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

[乃们还是直接看官网吧](https://link.jianshu.com/?t=https://hexo.io/zh-cn/docs/)

#### Hexo安装配置

#### 安装Hexo

因为虚拟主机不需要使用git， 所以直接
安装hexo

```shell
$ npm install -g hexo-cli  
```

#### 新建一个项目

```shell
$ mkdir newHexo
$ cd newHexo
$ hexo init  
```

此时已经可以在本地看到效果了

```shell
$ hexo server 
```

浏览器中输入 `http://localhost:4000`

### 2.3、Hexo 编写第一篇博客

#### Hexo 编写第一篇博客

```
$ hexo new "文章标题"
```

会在 `source/_post` 生成一个md文档， 使用你自己喜欢的md编辑工具打开，就可以编辑了，当然也可以直接把你在简书的文章复制过来
 需要注意的是， 使用中文名称会导致错误，但是不用担心文章标题不能为中文，在里面呢

```
title: Hexo与阿里云虚拟主机搭建博客
date: 2017-06-01 15:07:02
tags:
```

在你的浏览器刷新`http://localhost:4000`，就可以看到效果

### 2.4、Hexo deploy配置与部署到虚拟主机

发布到虚拟主机有很多方法，我们当然挑简单的来

#### VPS上需要安装vsftp

参考：https://www.cnblogs.com/leoxuan/p/8329998.html

如果需要root登录，需要打开root登录配置

#### 配置Hexo

[官方文档配置说明](https://link.jianshu.com/?t=https://hexo.io/zh-cn/docs/configuration.html)
设置博客标题，作者，主题等信息已经足够详细了，我主要说一下 deploy部分,使用FTP的方式把网站内容推送到虚拟主机上

#### update：

新版hexo取消了默认安装ftpsync， 会报错 `ERROR Deployer not found: ftpsync` 手动安装即可

```ruby
$ npm install hexo-deployer-ftpsync --save
deploy:
  type: ftpsync
  host: bxu******.my3w.com //主机地址
  user: bxu******          //用户名
  pass: *********          //密码
  remote: /htdocs          //目录，应该所有阿里云虚拟主机的网站内容目录都是这个，不是根目录 /
  port: 
  ignore: .DS_Store
  connections: 
  verbose: true 
```

发布不成功，检查单独连接ftp服务器是否成功。

#### 生成发布文件

```
$ hexo generate
```

此时如果你要手动部署（那我们还配置deploy搞毛用）， 使用你的FTP工具直接将 public 下的文件放进 虚拟主机的 htdocs 目录下，刷新你的网站，就看到效果了

#### 一键发布

```
$ hexo deploy
```

刷新网站， 就可以看到你写的东西了

#### 发布可能遇到的错误

参考：https://www.jianshu.com/p/da941bd0a3dd

```shell
因hexo搭建也有一段时间了，刚开始部署的时候，直接部署在了服务器上，还是有些不方便，每次写完都要vi。后来查查文档，发现了几种部署到服务器端的方法，自己特意找了一种试试，记录一下，方便自己，尽量方便大家。

注：
本教程指next主题部署文件至服务器，不是Git。

官方提供了几种部署方案，我这里选择了ftpsync,至于为什么，只是因为我在用rsync遇到了一个比较严重的问题没有解决。等我研究明白了，再来补充。

ftpsync
以上是文档里提到的ftpsync的配置项，我看到有Blog上说这个插件会将本地目录里的所有文件同步成服务器一样的，但经过我测试，上传的时候只会上传public文件夹。所以ingore配置项你可以默认，由于我是将文件上传到服务器，为了方便所以我专门在我的服务器上创建了新用户专用作ftp上传：
adduser ***//自己设置名称

然后会提示你设置密码，然后一直默认设置就行了。这个命令比
useradd ***
useradd *** pass
方便多了
创建账号之后需要apt-get install vsftpd，否则无法用ftp登录服务器。
我服务器是ubuntu,其余操作系统请自行搜索。

error1
将配置文件_confiig.yml配置完成后，然后想着开心的

hexo g -d

然后报错 530 incorrect login

无论是在ftp里登录还是别的地方都报这个错误，没办好，只好先找度娘，最后不知道在哪位大哥的博客下找到了解决方案，只不过写的时候已经找不到那个链接了。报这个错误的主要原因是因为服务器端的vsftpd文件报错了：

解决方法

$ sudo -s
$ vi /etc/vsftpd.conf
$ //找到 #write_enable=YES 将前面的#去掉
error2
登录的问题解决了，其实心里还在打鼓，果不其然：

$ hexo clean
$ hexo g -d
还是报错，当时的错误代码没有截图，以致于现在不能给大家上原图了。但大致源码我还是可以写下来，大家自己对照吧。

Colletting:
Errors events.js:160
        throw: ***
        
        TCP: ***
        onread: ***
出现这个问题，建议大家还是找一下node的问题吧，我找了许久的答案最后也没能够说个所以然来,我当时的Node6.9.1。后来我是将自己的node重装了一遍，而且装的是Node4.7.0版本。这个问题就自动解决好了，如果有人知道，希望能不吝赐教。

error3
经历了前两个错误，我知道第三个错误也是避免不了的。再次部署hexo g -d遇到:
premission denied 550

一般操作过Linux系统的人都会知道这是因为缺少了权限，所以我先将服务器上的接收目录的权限放到了最大
$ chmod -R 777 ***//目录名
可依然还是报错550

MKDIR fail
      ***
550 : Cannot create a file when that file already exists
找来找去最后找到了这个大哥的Blog下：从他的Git下拉了一个src/parse.js文件修复了这问题。

踩坑道人Blog

然后一路开心的hexo g -d
没报错，然后打开自己的的博客，哈哈显示正常，然后再点文章：

然后

error4
nginx 403 Forbidden
心里当真是问候了很多句苍天。没办法，继续折腾呗。我没有上面这位大哥厉害，console了三个小时，耐心地找了会百度就找到了一个解决方案：

$ vi /etc/nginx.conf
//在第一行的位置有个user 将其修改为你的ftp账号如下：
//修改 ftp账号
$ user ftpuser
$ worker_processes
//重启Nginx:
$ service nginx restart
问题大概就这么多，欢迎交流指正。

```



# 二、Hexo博客系统

参考：https://www.jianshu.com/p/21c94eb7bcd1

## 1、建站

之后就可以在电脑里新建一个文件夹来作为存放博客全部内容的大本营了。我们直接用hexo命令来初始化博客文件夹：

```
hexo init <folder>
cd <folder>
npm install
```

`<folder>`就是文件夹的名字，我们可以自己随意取这个名字，我的经验是，现在初始化应该不需要后面`npm install`这个步骤了，在创建的时候 ，文件夹初始化已经把需要的内容都下载进去了。

文件夹开始初始化了

#### 1.1、站内内容

新建好的文件夹目录如下：

```ruby
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

这里解释一下各个文件夹的作用：

#####  config.yml

博客的配置文件，博客的名称、关键词、作者、语言、博客主题...设置都在里面。

##### package.json

应用程序信息，新添加的插件内容也会出现在这里面，我们可以不修改这里的内容。

##### scaffolds

scaffolds就是脚手架的意思，这里放了三个模板文件，分别是新添加博客文章（posts）、新添加博客页（page）和新添加草稿（draft）的目标样式。

这部分可以修改的内容是，我们可以在模板上添加比如categories等自定义内容

##### source

source是放置我们博客内容的地方，里面初始只有两个文件夹，一个是drafts（草稿），一个posts（文章），但之后我们通过命令新建tags（标签）还有categories（分类）页后，这里会相应地增加文件夹。

##### themes

放置主题文件包的地方。Hexo会根据这个文件来生成静态页面。

初始状态下只有landscape一个文件夹，后续我们可以添加自己喜欢的。

### 1.2、Hexo命令

##### `init`

新建一个网站。

```
hexo init <folder>
```

##### `new`

新建文章或页面。

```
hexo new <layout> "title"
```

这里的`<layout>`对应我们要添加的内容，如果是`posts`就是添加新的文章，如果是`page`就是添加新的页面。

默认是添加`posts`。

然后我们就可以在对应的posts或drafts文件夹里找到我们新建的文件，然后在文件里用Markdown的格式来写作了。

##### `generate`

生成静态页面

```shell
hexo generate
```

也可以简写成

```shell
hexo g
```

##### `deploy`

将内容部署到网站

```shell
hexo deploy
```

也可以简写成

```shell
hexo -d
```

##### `publish`

发布内容，实际上是将内容从drafts（草稿）文件夹移到posts（文章）文件夹。

```
hexo publish <layout> <filename>
```

##### `server`

启动服务器，默认情况下，访问网站为`http://localhost:4000/`

```
hexo server
```

也可以简写成

```
hexo s
```

> 根据我的经验，除了第一次部署的时候，我们会重点用到`hexo init`这个命令外，在平时写博客和发布过程中最常用的就是：
>
> -  `hexo n <filename>` 新建文章
> -  `hexo s` 启动服务器，在本地查看内容
> -  `hexo g` 生成静态页面
> -  `hexo deploy` 部署到网站
>
> 以上四个步骤。

其实以上命令我觉得就足够了，文档里还有很多功能，但我在实际使用的过程中都还没有遇到。

搭建好后我们在`localhost:4000`就可以看到这样的博客内容

### 1.3、实际操作

我在新建博客之后，做了以下改动：

#### 1、 创建“分类”页面

- 新建分类页面

  ```shell
  hexo new page categories
  ```

  给分类页面添加类型

  我们在source文件夹中的categories文件夹下找到index.md文件，并在它的头部加上type属性。

  ```shell
  ---
  title: 文章分类
  date: 2017-05-27 13:47:40
  type: "categories"   #这部分是新添加的
  ---
  ```

  给模板添加分类属性

  现在我们打开scarffolds文件夹里的post.md文件，给它的头部加上`categories:`，这样我们创建的所有新的文章都会自带这个属性，我们只需要往里填分类，就可以自动在网站上形成分类了。

  ```shell
  title: {{ title }}
  date: {{ date }}
  categories:
  tags:
  ```

  给文章添加分类

  现在我们可以找到一篇文章，然后尝试给它添加分类

  ```shell
  layout: posts
  title: 写给小白的express学习笔记1： express-static文件静态管理
  date: 2018-06-07 00:38:36
  categories: 学习笔记
  tags: [node.js, express]
  ```

#### 2、创建“标签”页面

创建"标签"页的方式和创建“分类”一样。

- 新建“标签”页面

  ```shell
  hexo new page tags
  ```

- 给标签页面添加类型

  我们在source文件夹中的tags文件夹下找到index.md文件，并在它的头部加上type属性。

  ```shell
  title: tags
  date: 2018-08-06 22:48:29
  type: "tags" #新添加的内容
  ```

- 给文章添加标签

  有两种写法都可以，第一种是类似数组的写法，把标签放在中括号`[]`里，用英文逗号隔开

  ```shell
  layout: posts
  title: 写给小白的express学习笔记1： express-static文件静态管理
  date: 2018-06-07 00:38:36
  categories: 学习笔记
  tags: [node.js, express]
  ```

  第二种写法是用`-`短划线列出来

  ```shell
  layout: posts
  title: 写给小白的express学习笔记1： express-static文件静态管理
  date: 2018-06-07 00:38:36
  categories: 学习笔记
  tags: 
  - node.js
  - express
  ```

### 1.4、部署域名

由于我是在VPS上搭建的，所以就不在github上弄了。

#### 部署Github

- 首先你必须有一个github账号

- 然后新建一个仓库，这一有第一个坑，我之前用了`hexoblog`来作为项目名称，一直没能搭建成功，后来看到其他大牛的经验，才发现项目名一定要是`用户名.github.io`的形式(README.md可选可不选)



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-6eda78cde5d219b2.jpg)

  image-20180809153134467

- 然后在setting里添加生成页面的选项



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-cc1d74abf3998381.jpg)

  image-20180809153304980



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-2266e03ca0151a30.jpg)

  image-20180809153343362

- 这个时候github页面其实就生成好了，但是我们的内容还需要同步到github上，所以打开hexo文件夹里的配置文件config.yml，添加部署路径



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-b8db29dceebd3dc2.jpg)

  image-20180809153610047

  这里注意两小点：

  - 属性和内容之间一定要有一个空格，配置文件有自己的格式规范
  - 如果你之前没有用git关联过自己的github库，需要配置SSH等参数，否则无法成功，这部分搜git就有很多相关教程

- 我们再用`hexo g && hexo deploy`就能将内容推送到github上了，在github页面上也能看到自己的内容了



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-7ea12cb3505f9a17.jpg)

  image-20180809153933270

#### 部署自己的域名

- 首先我们需要获取一个域名，我是在阿里云上购买了，上面可以根据自己想要的内容搜，比如我用了自己的名字，推荐给你的域名根据后缀不同会有价格上的区别，我选了一个不太贵的；

- 购买域名之后需要实名认证，这是另一个坑，我之前不知道实名认证审核完成前域名无法用，一直以为自己搭建失败了；

- 认证成功后需要解析域名



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-18aa68ed0f0fc617.jpg)

  image-20180809154942783



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-ed610e41e47a4b97.jpg)

  image-20180809155013659

  记录类型选CNAME，记录值是自己github生成页面的地址。

- 在博客的页面添加CNAME文件，并在里面记录自己域名的地址，将这个文件放在public文件夹下

- 这里还有一个小坑，CNAME文件经常被覆盖，导致我们重新部署博客后，链接就不可用了，这里可以下载一个叫`hexo-generator-cname`的插件，这样它会自动搞定CNAME的问题，只需要第一次手动将域名添加到文件里即可

  ```shell
  npm i hexo-generator-cname --save
  ```

- 最后`hexo g && hexo deploy`就可以了



### 1.5、删除文章

参考：https://blog.csdn.net/time888/article/details/70249241

删除文章的过程一样也很简单，先删除本地文件，然后通过生成和部署命令进而将远程仓库中的文件也一并删除。具体来说，以最开始默认形成的helloworld.md这篇文章为例。

首先进入到source / _post 文件夹中，找到helloworld.md文件，在本地直接执行删除。然后依次执行hexo g，hexo d，再去主页查看你就会发现你的博客上面已经空空如也了，这就是如何删除文章的方法。



## 2、一些主题

- 好看不适用：https://github.com/WongMinHo/hexo-theme-miho
- 适用色彩淡：https://github.com/chaooo/hexo-theme-BlueLake
- 适用窗口乱，俗：https://github.com/Jamling/hexo-theme-nova
- 动态适用，栏目不好：https://qutang.github.io/
- 太能动了吧：https://github.com/objectyan/hexo-theme-hannah
- 略乱，适用：https://github.com/AlynxZhou/hexo-theme-aria
