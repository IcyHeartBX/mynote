---
title: Hexo主题
categories:
  - blog
tags:
  - blog
  - 主题
date: 2018-10-08 10:24:14
cover_picture: https://ws2.sinaimg.cn/large/006tNbRwly1fw47vwd01yj30fa03mmx2.jpg
---

# 一、NexT主题

参考：https://www.jianshu.com/p/21c94eb7bcd1

hexo有很多开源的主题，我选了[NexT](https://github.com/iissnan/hexo-theme-next)，开始只是觉得很简洁清爽，后来发现它的功能挺齐全的，提前解决了很多搭建过程中会遇到的问题。这里强烈推荐一下。

首先，NexT也有[中文文档](https://github.com/iissnan/hexo-theme-next/blob/master/README.cn.md)，然后我们就可以开始了。

#### 安装

我是用的`git clone`的方法，文档中还有其他方法

```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

#### 设置主题

在**hexo根目录**下的配置文件config.yml里设置主题

```
theme: next
```

#### 配置主题

接下来我们就可以来按需配置主题内容了，所有内容都在**themes/next**文件夹下的config.yml文件里修改。

官方文档里写的是有些配置需要将一部分代码添加到配置文件中，但其实不用，我们逐行看配置文件就会发现，有很多功能都已经放在配置文件里了，只是注释掉了，我们只需要取消注释，把需要的相关信息补全即可使用

##### 菜单栏 `menu` 

原生菜单栏有主页、关于、分类、标签等数个选项，但是在配置文件中是注释掉的状态，这里我们自行修改注释就行

```
menu:
  home: / || home
  # about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  # schedule: /schedule/ || calendar
  # sitemap: /sitemap.xml || sitemap
  # commonweal: /404/ || heartbeat
```

注意点：

- 如果事先没有通过`hexo new page <pageName>`来创建页面的话，即使在配置文件中取消注释，页面也没法显示
- 我们也可以添加自己想要添加的页面，不用局限在配置文件里提供的选择里
-  `||`后面是fontAwesome里的文件对应的名称
-  `menu_icons`记得选`enable: true`（默认应该是`true`）

我在这部分添加了两个自定义的页面，后面在第三方插件部分我会再提到。

```
menu:
  home: / || home
  # about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  读书: /books || book
  电影: /movies || film
  archives: /archives/ || archive
  # schedule: /schedule/ || calendar
  # sitemap: /sitemap.xml || sitemap
  # commonweal: /404/ || heartbeat
```

##### 主题风格 `schemes` 

主题提供了4个，我们把想要选择的取消注释，其他三个保持注释掉的状态即可。

- `Muse`



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-5e7193faf3720112.jpg)

  image-20180809164700600

- Mist



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-dbd774ea0be0fe87.jpg)

  image-20180809164749052

- Pisces



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-327385996d44bb02.jpg)

  image-20180809164925685

- Gemini



  ![img](https:////upload-images.jianshu.io/upload_images/9240001-0e58f7644c380210.jpg)

  image-20180809165023401

选择主题后也可以自定义，不过我还没摸清楚有哪些地方可以自定义，等弄清楚了我再来更新。

##### 底部建站时间和图标修改

修改主题的配置文件：

```
footer:
  # Specify the date when the site was setup.
  # If not defined, current year will be used.
  since: 2018

  # Icon between year and copyright info.
  icon: snowflake-o

  # If not defined, will be used `author` from Hexo main config.
  copyright:
  # -------------------------------------------------------------
  # Hexo link (Powered by Hexo).
  powered: false

  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: false
    # Version info of NexT after scheme info (vX.X.X).
    # version: false
```

我在这部分做了这样几件事：

- 把用户的图标从小人`user`改成了雪花`snowflake-o` 
-  `copyright`留空，显示成页面`author`即我的名字
-  `powered: false`把hexo的授权图片取消了
-  `theme: enable:false` 把主题的内容也取消了

这样底部信息比较简单。



![img](https:////upload-images.jianshu.io/upload_images/9240001-45bf7fb855b8e642.jpg?)

image-20180809172835606

##### 个人社交信息 `social` 

在`social`里我们可以自定义自己想要在个人信息部分展现的账号，同时给他们加上图标。

```
social:
  GitHub: https://github.com/XuQuan-nikkkki || github
  E-Mail: mailto:xuquan1225@hotmail.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
```

注意点：

-  `||`后面对应的名称是fontAwesome里图标的名称，如果我们选择的账号没有对应的图标（如豆瓣、知乎），我们可以在fontAwesome库里去选择自己喜欢的图标
- 建议不要找太新的fontAwesome图标，主题关联的库版本没有那么新，很可能显示不了或者显示一个地球

##### 网站动画效果

为了网站响应速度我们可以把网站的动画关掉

```
motion:
  enable: false
```

但我觉得页面比较素，所以开了动画，选择了`canvas-nest`这一个，主题自带四种效果，可以选自己喜欢的。

```
motion:
  enable: true
  async: true
  
# Canvas-nest
canvas_nest: true

# three_waves
three_waves: false

# canvas_lines
canvas_lines: false

# canvas_sphere
canvas_sphere: false
```

##### 评论系统

NexT原生支持多说、Disqus、hypercomments等多种评论系统。我选择了Disqus。

方法也非常简单。直接去Disqus注册，注册完了在配置的时候会给你一个名为shortname的ID，将这个ID填在配置文件里即可。

```
# Disqus
disqus:
  enable: true
  shortname: xuquan
  count: true
```

##### 统计文章字数和阅读时间

```
post_wordcount:
  item_text: true
  wordcount: true  # 文章字数
  min2read: true   # 阅读时间
  totalcount: true  # 总共字数
  separated_meta: true
```

##### 统计阅读次数

这里我用的是leancloud的服务，具体方法参考NexT上的[教程](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud),添加完之后效果如下：



![img](https:////upload-images.jianshu.io/upload_images/9240001-f06f60721b93eb65.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/106/format/webp)

image-20180809175133462

### 第三方插件

#### Hexo-admin

Hexo-admin插件允许我们直接在本地页面上修改文章内容。

- 下载

  ```
  npm i hexo-admin --save
  ```

- 登录`http://localhost:4000/admin`即可看到我们所有的文章内容，并且在可视化界面中操作文章内容

#### Hexo-douban

[`hexo-douban`](https://github.com/mythsman/hexo-douban)插件可以在博客中添加豆瓣电影、读书和游戏页面，关联我们自己的账号。

- 下载

  ```
  npm install hexo-douban --save
  ```

- 配置

  在**hexo根目录**下的config.yml文件中添加如下内容

  ```
  douban:
    user: 
    builtin: false
    book:
      title: 'This is my book title'
      quote: 'This is my book quote'
    movie:
      title: 'This is my movie title'
      quote: 'This is my movie quote'
    game:
      title: 'This is my game title'
      quote: 'This is my game quote'
    timeout: 10000 
  ```

  title和quote后面的内容会分别作为电影/读书/游戏页面的标题和副标题（引言）呈现在博客里。

  `user`就写我们豆瓣的id，可以在“我的豆瓣”页面中找到，`builtin`指是否将生成页面功能嵌入`hexo s`和`hexo g`中，建议选`false`，因为`true`会导致页面每次启动本地服务器都需要很长时间生成豆瓣页面，长到怀疑人生。

- 生成页面

  ```
  hexo douban   #生成读书、电影、游戏三个页面
  hexo douban -b  #生成读书页面
  hexo douban -m  #生成电影页面
  hexo douban -g  #生成游戏页面
  ```

- 在博客中生成页面

  这里就需要用到我们前面提过的`hexo new`命令了。

  ```
  hexo new page books
  hexo new page movies
  hexo new page games
  ```

- 在博客中添加页面

  在`menu`部分添加我们需要添加的页面名称和相对路径

  ```
  menu:
    Home: /
    Archives: /archives
    Books: /books     #This is your books page
    Movies: /movies   #This is your movies page
    Games: /games   #This is your games page
  ```

- 部署到博客

  ```
  hexo g && hexo deploy
  ```

### 我踩过的坑

#### iPic图片上传

hexo博客发布Typora写好的内容也会出现图片无法同步的问题，网上有大佬给出的解决方案是使用`hexo-asset-image`插件，这样在创建博客时会有一个与`.md`文件同名的文件夹，将图片同步到文件夹内即可。

但时间下来还是比较麻烦，因为Typora并没有自定义图片路径的功能，它会放在与文件相关的asset文件夹内。

我找到的最终方案是使用Typora自带的一个功能：图片上传iPic图床。这样在添加图片的时候，图片链接就自动更换成了图床的地址，这时同步到博客就没有问题了。

#### 评论系统

因为多说已经停止服务了，最开始看到有人说Disqus得翻墙，就选了一个韩国的评论服务，叫来必力，但事实证明墙外就没有稳定的服务，在我挂VPN的情况下也要加载好半天，后来就还是换成了Disqus，具体配置方法看前文。



# 二、NexT主题优化

参考：https://www.jianshu.com/p/1f8107a8778c

在Hexo中有两个很重要的名为`_config.yml`的文件，其中一个在站点安装的根目录下，另一个在主题目录下。前者提供了Hexo自身的一些基本配置信息，后者为你所安装的主题的相关配置。为了方便区分，我们把前者称为**站点配置文件**，后者称为**主题配置文件**。

## 1、站点配置文件

文件路径`站点根目录/_config.yml`，编辑软件推荐使用Sublime Text 。
这里贴一下个人的部分配置，可以改一下相关内容自行体会一下效果：

```shell
# Site
title: Alvabill
subtitle: Stay Hungry, Stay Foolish
author: Alvabill
description: "Alvabill个人站，主要涉及前端知识共享、实践教程、前沿技术共同学习等方面"  #网站描述 SEO
language: en
timezone: Asia/Shanghai

```

- “title”：博客的名称。
-  “subtitle”：根据主题的不同，有的会显示有的不会显示。
-  “description”：主要用于SEO，告诉搜索引擎一个关于站点的简单描述，通常建议在其中包含网站的关键词。
-  “author”：作者名称，用于主题显示文章的作者。
-  “language”：语言会对应的解析正在应用的主题中的languages文件夹下的不同语言文件。所以这里的名称要和languages文件夹下的语言文件名称一致。
-  “timezone”：可不填写。

```shell
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://alvabill.ml
root: /
permalink: :title/
permalink_defaults:


```

- “url”：一般填写自己的站点链接。
-  “root”：设置根目录的位置。如果你的网站存放在子目录中，例如 `http://yoursite.com/blog`，则应该将你的 url 设为`http://yoursite.com/blog` 并把 root 设为 `/blog/`。
-  “permalink”：生成的链接的格式。带井号的是默认的格式，带有日期感觉怪怪的，改成了自己喜欢的格式。规则也比较简单，标签前面要加英文冒号。
-  “permalink_defaults”:  生成链接中各部分的默认值

```shell
# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
  - README.md
  - CNAME

```

目录一般不需要修改，这里需要改动的是`skip_render`，跳过指定文件的渲染，这里写上去着两个文件名便可，在上一篇文章中已经详细描述过这里就不累赘了。

```shell
# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

```

书写相关的设置

- “new_post_name”：新的博文的文件名
-  “default_layout:“ 默认布局
-  “filename_case: 0“ #把文件名称转换为 (1) 小写或 (2) 大写
-  “render_drafts: false“ 是否显示草稿
-  “post_asset_folder: false“ #是否启动资源文件夹
-  “relative_link: false“ #把链接改为与根目录的相对位址
-  “future: true “
-  “highlight:“ #代码块的设置，Hexo自带的代码高亮插件

```shell
# Category & Tag
default_category: uncategorized
category_map:
tag_map:
```

分类和标签的设置

- “default_category”：如果撰写文章时没有设置分类，默认的分类选择。
- “category_map”：用于映射分类的别名。
- “tag_map”：用法和分类别名是一样的。

```shell
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository:
    github: git@github.com:Alvabill/Alvabill.github.io.git,master
    coding: git@git.coding.net:Alvabill/Alvabill.git,master

```

## 2、主题配置文件

文件路径`站点根目录/themes/next/_config.yml`，编辑软件推荐使用Sublime Text 。
这里先选择主题：

```shell
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes  # NexT 主题提供三种布局
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini

```

主题选择，在前面加`#`注释掉其他的，这里我们选Mist，其他主题你们也可以体验一下，不过不保证本教程的优化全部适配哦，不过选择其他主题的小伙伴遇到的问题也欢迎在评论区提出一起交流。

### 2.1、添加RSS

在**主题配置文件**中有NexT默认的RSS设置，默认为留空，这时使用 Hexo 生成的 Feed 链接，需要先安装 [hexo-generator-feed](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fhexojs%2Fhexo-generator-feed)插件。
在站点根目录打开git bash，安装插件：

```shell
npm install --save hexo-generator-feed
```

修改**站点配置文件**，在最后添加以下代码：

```shell
feed: # RSS订阅插件
  type: atom
  path: atom.xml
  limit: 0

plugins: hexo-generate-feed
```

修改**主题配置文件**如下：

```shel
# Set rss to false to disable feed link.
# Leave rss as empty to use site's feed link.
# Set rss to specific value if you have burned your feed already.
rss: /atom.xml

```

### 2.2、添加标签、分类等页面

1、 修改**主题配置文件**，在菜单项添加以下内容

```shell
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------

# When running the site in a subdirectory (e.g. domain.tld/blog), remove the leading slash (/archives -> archives)
menu:
  home: /
  archives: /archives/
  tags: /tags/
  categories: /categories/
  about: /about/
  #sitemap: /sitemap.xml
  #commonweal: /404/

```

![](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519391446050.jpg)



这里可以添加图标，个人不是很喜欢所以没有添加，图标的代码就在以上代码的下边，可以自行修改体验。

2、 新建页面
在站点根目录输入以下代码新建页面：

```shell
$ hexo new page tags
$ hexo new page categories
$ hexo new page about
```

> 如果有集成评论服务，页面也会带有评论。因为后面我们会添加评论系统所以这里需要添加字段 comments 并将值设置为 false。

### 2.3、设置网站icon

**主题配置文件**中第一行代码就是网站icon设置，这里你只需要找到你喜欢的logo把它制作成ico格式然后改名`favicon.ico`，放到`/themes/next/source/images`下面即可。

```shell
# Put your favicon.ico into `hexo-site/source/` directory.
favicon: /favicon.ico # 网站logo
```

### 2.4、添加侧边栏社交链接

主要修改**主题配置文件**的社交链接和对应图标5

```shell
# Social Links
# Key is the link label showing to end users.
# Value is the target link (E.g. GitHub: https://github.com/iissnan)
social:  # 添加你的社交账号
  #LinkLabel: Link
  GitHub: https://github.com/Alvabill
  fcc: https://www.freecodecamp.cn/alvabill
  简书: https://www.jianshu.com/u/439a6eee60e1
  CSDN: http://blog.csdn.net/weixin_38796712
# Social Links Icons
# Icon Mapping:
#   Map a menu item to a specific FontAwesome icon name.
#   Key is the name of the item and value is the name of FontAwesome icon. Key is case-senstive.
#   When an globe mask icon presenting up means that the item has no mapping icon.
social_icons:
  enable: true
  icons_only: false
  transition: false
  # Icon Mappings.
  # KeyMapsToSocialItemKey: NameOfTheIconFromFontAwesome
  fcc: free-code-camp
  GitHub: github
  简书: book
  CSDN: rotate-right
```

实现效果如下：



![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519394054058.jpg)

### 2.5、添加侧边栏友情链接

修改**主题配置文件**：

```
# Blog rolls  推荐阅读
links_title: Links
#links_layout: block
links_layout: inline
links:
  Web前端导航: http://www.alloyteam.com/nav/
  创造狮导航: http://www.chuangzaoshi.com/code
  前端书籍资料: http://www.36zhen.com/t?id=3448
  掘金酱: http://e.xitu.io/
  V2EX: https://www.v2ex.com/
  印记中文: https://www.v2ex.com/
```

实现效果：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519394239052.jpg)

### 2.6、底部显示建站时间和图标修改

修改**主题配置文件**：

```
# Specify the date when the site was setup
since: 2018 # 建站年份

# icon between year and author @Footer
authoricon: snowflake-o
```

实现效果：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519394523529.jpg)

> 雪花图标需要用到最新的fa图标库，后面会介绍如何设置默认库。



### 2.7、微信支付宝打赏功能

在**主题配置文件**中的微信or支付宝收款地址处填入收款二维码图片即可：

```
# Reward
#reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
wechatpay: http://p3dm71oa7.bkt.clouddn.com/wcpay.png
alipay: http://p3dm71oa7.bkt.clouddn.com/zfbpay.jpg
#bitcoin: /images/bitcoin.png
```

实现效果：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519395390692.jpg)

### 2.7、关闭网站动画效果

为了追求更快的响应速度我们可以把网站的大部分动画关掉，这里修改**主题配置文件**的一行代码即可：

```
# Motion
use_motion: [false/true]
```

### 2.8、设置第三方JS库

在**主题配置文件**中设定成合适的 CDN 地址，此特性可以加速静态资源（JavaScript 第三方库）的加载。
例如：

````shell
# Script Vendors.
# Set a CDN address for the vendor you want to customize.
# For example
#    mquery: https://ajax.googleapis.com/ajax/libs/jquery/2.2.0/jquery.min.js
# Be aware that you should use the same version as internal ones to avoid potential problems.
# Please use the https protocol of CDN files when you enable https on your site.
vendors:
  # Internal path prefix. Please do not edit it.
  _internal: lib

  # Internal version: 2.1.3
  jquery: //cdn.jsdelivr.net/jquery/2.1.3/jquery.min.js

  # Internal version: 2.1.5
  # See: http://fancyapps.com/fancybox/
  fancybox: //cdn.jsdelivr.net/fancybox/2.1.5/jquery.fancybox.pack.js
  fancybox_css: //cdn.jsdelivr.net/fancybox/2.1.5/jquery.fancybox.min.css

  # Internal version: 1.0.6
  # See: https://github.com/ftlabs/fastclick
  fastclick: //cdn.jsdelivr.net/fastclick/1.0.6/fastclick.min.js

  # Internal version: 1.9.7
  # See: https://github.com/tuupola/jquery_lazyload
  lazyload: //cdn.jsdelivr.net/jquery.lazyload/1.9.3/jquery.lazyload.min.js

  # Internal version: 1.2.1
  # See: http://VelocityJS.org
  velocity: //cdn.jsdelivr.net/velocity/1.2.3/velocity.min.js

  # Internal version: 1.2.1
  # See: http://VelocityJS.org
  velocity_ui: //cdn.jsdelivr.net/velocity/1.2.3/velocity.ui.min.js

  # Internal version: 0.7.9
  # See: https://faisalman.github.io/ua-parser-js/
  ua_parser: //cdn.jsdelivr.net/ua-parser.js/0.7.10/ua-parser.min.js

  # Internal version: 4.6.2
  # See: http://fontawesome.io/
  fontawesome: //maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css

  # Internal version: 1
  # https://www.algolia.com
  algolia_instant_js:
  algolia_instant_css:

  # Internal version: 1.0.2
  # See: https://github.com/HubSpot/pace
  # Or use direct links below:
  # pace: //cdn.bootcss.com/pace/1.0.2/pace.min.js
  # pace_css: //cdn.bootcss.com/pace/1.0.2/themes/blue/pace-theme-flash.min.css
  pace: //cdn.bootcss.com/pace/1.0.2/pace.min.js
  pace_css: //cdn.bootcss.com/pace/1.0.2/themes/blue/pace-theme-flash.min.css

  # Internal version: 1.0.0
  # https://github.com/hustcc/canvas-nest.js
  canvas_nest: //cdn.bootcss.com/canvas-nest.js/1.0.1/canvas-nest.min.js

````

### 2.9、添加评论系统

NexT支持的第三方的评论系统有很多，不过不少已经关闭不再可用了，而且对于国内来说比较友好的现在应该就只有[来必力](https://link.jianshu.com?t=https%3A%2F%2Flivere.com)，当然喜欢折腾的小伙伴可以尝试一下其他的或者自建评论系统。这里就先介绍目前最简单可行的方案，也就是来必力。

获取来必力id：
 登陆 [来必力](https://link.jianshu.com?t=https%3A%2F%2Flivere.com%2F) 注册获取，这里要注意，这个韩国的系统注册啥的真的太慢了，所以要记住不要耐不住关闭页面或者狂刷新，耐心等待就好。
 注册后点击导航上边的安装，选择免费的city版本：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519486026113.jpg)

点击现在安装后填入网站的一些信息就可以获取到安装代码，框中的就是你的来必力id：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519486165098.jpg)

复制上边的id，在**主题配置文件**里面搜索`livere_uid`，在后面添加来必力id即可：

```
# Support for LiveRe comments system.
# You can get your uid from https://livere.com/insight/myCode (General web site)
livere_uid: {你的来必力id}
```

另外可以点击用户头像进入管理界面个性化你的评论系统：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519486309368.jpg)



最终实现效果:

 ![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519486370273.jpg)

### 2.10、统计访客量以及文章阅读量

NexT主题集成了不蒜子统计功能：

```shell
# Show PV/UV of the website/page with busuanzi.
# Get more information on http://ibruce.info/2015/04/04/busuanzi/
# 不蒜子统计功能
busuanzi_count:
  # count values only if the other configs are false
  enable: false
  # custom uv span for the whole site
  site_uv: false
  site_uv_header: <i class="fa fa-user"></i>
  site_uv_footer:
  # custom pv span for the whole site
  site_pv: false
  site_pv_header: <i class="fa fa-eye"></i>
  site_pv_footer:
  # custom pv span for one page only
  page_pv: false
  page_pv_header: <i class="fa fa-file-o"></i>
  page_pv_footer:

```

- 当`enable: true`时，代表开启全局开关。若`site_uv`、`site_pv`、`page_pv`的值均为`false`时，不蒜子仅作记录而不会在页面上显示。

-  当`site_uv: true`时，代表在页面底部显示站点的UV值。

-  当`site_pv: true`时，代表在页面底部显示站点的PV值。

-  当`page_pv: true`时，代表在文章页面的标题下显示该页面的PV值（阅读数）。

-  `site_uv_header`和`site_uv_footer`这几个为自定义样式配置，相关的值留空时将不显示，可以使用（带特效的）font-awesome。

  示例：

```
enable: true
# 效果：本站访客数12345人次
site_uv: true
site_uv_header: 本站访客数
site_uv_footer: 人次
# 效果：本站总访问量12345次（一般不开启这个）
site_pv: true
site_pv_header: 本站总访问量
site_pv_footer: 次
# 效果：本文总阅读量12345次
page_pv: true
page_pv_header: 本文总阅读量
page_pv_footer: 次

```



### 2.11、访问数配置[LeanCloud](https://leancloud.cn/)

相比不蒜子的统计，[LeanCloud](https://link.jianshu.com/?t=https%3A%2F%2Fleancloud.cn%2F)的文章阅读量统计更加稳定靠谱，所以本人也把网站的文章内统计改为LeanCloud的了。
设置方法参考该文章--[传送门](https://link.jianshu.com/?t=https%3A%2F%2Fnotes.wanghao.work%2F2015-10-21-%25E4%25B8%25BANexT%25E4%25B8%25BB%25E9%25A2%2598%25E6%25B7%25BB%25E5%258A%25A0%25E6%2596%2587%25E7%25AB%25A0%25E9%2598%2585%25E8%25AF%25BB%25E9%2587%258F%25E7%25BB%259F%25E8%25AE%25A1%25E5%258A%259F%25E8%2583%25BD.html%23%25E9%2585%258D%25E7%25BD%25AELeanCloud)

实现效果：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519488690562.jpg)

在注册完成LeanCloud帐号并验证邮箱之后，我们就可以登录我们的LeanCloud帐号，进行一番配置之后拿到`AppID`以及`AppKey`这两个参数即可正常使用文章阅读量统计的功能了。

创建应用

- 我们新建一个应用来专门进行博客的访问统计的数据操作。首先，打开控制台，如下图所示：

![打开控制台](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/open_consoloe.png)

- 在出现的界面点击`创建应用`：

![创建应用](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/create_app.png)

- 在接下来的页面，新建的应用名称我们可以随意输入，即便是输入的不满意我们后续也是可以更改的:

![创建的新应用名称](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/creating_app.png)

- 这里为了演示的方便，我新创建一个取名为test的应用。创建完成之后我们点击新创建的应用的名字来进行该应用的参数配置：

![打开应用参数配置界面](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/create_class.png)

- 在应用的数据配置界面，左侧下划线开头的都是系统预定义好的表，为了便于区分我们新建一张表来保存我们的数据。点击左侧右上角的齿轮图标，新建Class：
  在弹出的选项中选择`创建Class`来新建Class用来专门保存我们博客的文章访问量等数据:
  点击`创建Class`之后，~~理论上来说名字可以随意取名，只要你交互代码做相应的更改即可~~，但是为了保证我们前面对NexT主题的修改兼容，此处的**新建Class名字必须为Counter**:

![权限配置](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/creating_class.png)

- 由于LeanCloud升级了默认的ACL权限，如果你想避免后续因为权限的问题导致次数统计显示不正常，建议在此处选择`无限制`。

![打开应用设置](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/open_app_key.png)

创建完成之后，左侧数据栏应该会多出一栏名为`Counter`的栏目，这个时候我们点击顶部的设置，切换到test应用的操作界面:
在弹出的界面中，选择左侧的`应用Key`选项，即可发现我们创建应用的`AppID`以及`AppKey`，有了它，我们就有权限能够通过主题中配置好的Javascript代码与这个应用的Counter表进行数据存取操作了:

![获取Appid、Appkey](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/opened_app_key.png)

复制`AppID`以及`AppKey`并在NexT主题的`_config.yml`文件中我们相应的位置填入即可，正确配置之后文件内容像这个样子:

```
leancloud_visitors:
  enable: true
  app_id: joaeuuc4hsqudUUwx4gIvGF6-gzGzoHsz
  app_key: E9UJsJpw1omCHuS22PdSpKoh
```

这个时候重新生成部署Hexo博客，应该就可以正常使用文章阅读量统计的功能了。需要特别说明的是：记录文章访问量的唯一标识符是文章的`发布日期`以及`文章的标题`，因此请确保这两个数值组合的唯一性，如果你更改了这两个数值，会造成文章阅读数值的清零重计。

**后台管理**

当你配置部分完成之后，初始的文章统计量显示为0，但是这个时候我们LeanCloud对应的应用的`Counter`表中并没有相应的记录，只是单纯的显示为0而已，当博客文章在配置好阅读量统计服务之后第一次打开时，便会自动向服务器发送数据来创建一条数据，该数据会被记录在对应的应用的`Counter`表中。

![后台管理](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/background.png)

我们可以修改其中的`time`字段的数值来达到修改某一篇文章的访问量的目的（博客文章访问量快递提升人气的装逼利器）。双击具体的数值，修改之后回车即可保存。

- `url`字段被当作唯一`ID`来使用，因此如果你不知道带来的后果的话请不要修改。
- `title`字段显示的是博客文章的标题，用于后台管理的时候区分文章之用，没有什么实际作用。
- 其他字段皆为自动生成，具体作用请查阅LeanCloud官方文档，如果你不知道有什么作用请不要随意修改。

**Web安全**

因为AppID以及AppKey是暴露在外的，因此如果一些别用用心之人知道了之后用于其它目的是得不偿失的，为了确保只用于我们自己的博客，建议开启Web安全选项，这样就只能通过我们自己的域名才有权访问后台的数据了，可以进一步提升安全性。

选择应用的设置的`安全中心`选项卡:

![进入安全中心](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/open_safe_center.png)

在`Web 安全域名`中填入我们自己的博客域名，来确保数据调用的安全:

![锁定域名](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/bind_domain.png)

如果你不知道怎么填写安全域名而或者填写完成之后发现博客文章访问量显示不正常，打开浏览器调试模式，发现如下图的输出:

![Web安全域名填写错误](http://7xkj6q.com1.z0.glb.clouddn.com/static/images/leancloud-page-anlysis/broswer_403.png)

这说明你的安全域名填写错误，导致服务器拒绝了数据交互的请求，你可以更改为正确的安全域名或者你不知道如何修改请在本博文中留言或者放弃设置Web安全域名。

Enjoy it！



### 2.12、字数统计

用于统计文章的字数以及分析出阅读时间。
 在**主题配置文件**中，搜索`wordcount`，设置为下面这样就可以了：

```shell
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  min2read: true
  wordcount: true
  separated_meta: true
```

再打开`\themes\next\layout\_macro\post.swig`文件，在`leancloud-visitors-count`后面位置添加一个分割符：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519489975360.jpg)



实现效果：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519489219127.jpg)



另外，在`/themes/next/layout/_partials/footer.swig`文件`endif %}`前加上下面代码可以实现在站点底部统计全站字数：

```
<div class="theme-info">
  <span class="post-count">Total Words:{{ totalcount(site) }}</span>
</div>
```

实现效果：





如果无法显示可能是`hexo-wordcount`插件没有安装，git bash在网站根目录安装一下就可以：

```
$ npm install hexo-wordcount --save
```



------



### 

### 2.13、添加分享文章功能

这里我们使用[AddThis](https://link.jianshu.com?t=https%3A%2F%2Fwww.addthis.com%2F)，为什么呢，晒一下我的样式你就知道了：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519547206460.jpg)



具体选择什么样式可以在AddThis网站上面设置。
 首先注册账号（需要科学上网），注册后找到该位置：

![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519547553087.jpg)



 在主题配置文件中搜索`add_this_id`，去掉前面的注释，添加上你的

```
AddThis ID
```

就可以了。



```
# Share  分享
#jiathis: true
# Warning: JiaThis does not support https. 博主实测支持https
add_this_id: {your AddThis ID}
```

具体添加方式如下，可以选择多种样式：

 ![img](http://p3dm71oa7.bkt.clouddn.com/%E5%B0%8F%E4%B9%A6%E5%8C%A0/1519547877122.jpg)





# 附录、

## 1、Hexo之next主题设置首页不显示全文(只显示预览)

参考：https://www.jianshu.com/p/393d067dba8d

###### 问题

- next是Hexo的一个博客主题，这个主题 ，首页显示的文章列表，每一遍都是全文，不方便概览。

###### 希望达到的效果

- 首页显示文章列表，列表里的每一篇文章只显示预览，不显示全文。

###### 解决方法

百度了好久，折腾了半天，改了各种参数，算是撞破了头，最后试出来了。
 哎，搞移动端的，不太懂网页的这些东西，不说废话了，直接上解决方法：

- 进入hexo博客项目的themes/next目录
- 用文本编辑器打开_config.yml文件
- 搜索"auto_excerpt",找到如下部分：

```shell
# Automatically Excerpt. Not recommand.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: false
  length: 150
```

- 把enable改为对应的false改为true，然后`hexo d -g`，再进主页，问题就解决了！





![img](https:////upload-images.jianshu.io/upload_images/937405-2f503d6c26d90198.png?imageMogr2/auto-orient/strip%7CimageView2)



