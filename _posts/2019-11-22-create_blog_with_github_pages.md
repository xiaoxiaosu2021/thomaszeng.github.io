---
layout: post
title: "可能是最全面的github pages搭建个人博客教程"
date:   2019-11-22
tags: [geek]
comments: true
author: lemonchann
---

作为一个程序员怎么能没有自己的个人博客呢，这里详细记录和分享我的博客搭建经验，让你轻轻松松拥有自己的博客网站。傻瓜式一站式教你用 github pages 来搭建博客，详细记录全过程，保证你能学会。

如果你是非程序员或者不关系技术细节，只需花 3 分钟阅读前面 5 个章节内容，就能轻松拥有自己的博客。

<!-- more -->

## 开始

话不多说，直接上图先来看下我的博客整体效果。[**点击在线预览我的博客**]( https://lemonchann.github.io/blog/)，个人比较喜欢这种简约的博客风格，不要花里胡哨但该有的功也都有。

![blogPage](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)



下面列举这个博客具有的功能特性，其中我比较看重归档和搜索能力。

### 支持特性

- 简约风格博客

- Powered By Jekyll

- 博客文章搜索

- 自定义社交链接

- 网站访客统计

- Google Analytics 网站分析

- Gitalk评论功能

- 自定义关于about页面

- 支持中文布局

- 支持归档与标签

- 支持改变主题颜色

- 支持添加文章目录

  

## 建立博客Git仓库

首先你要在[github](https://github.com/)上有自己博客仓库，用来生成和存放博客文章。你可以直接fork我的博客仓库。这样你马上有了自己的博客仓库。

[点这里我的博客地址](https://github.com/lemonchann/lemonchann.github.io)进去点击 fork，之后在你自己的仓库下会看到刚复制的仓库，以后的操作都在你自己的仓库进行，当然想感谢我写这个教程就帮我点个 start 吧！

![fork博客](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

**版权声明： fork之后_posts文件夹内容是我的博客文章，版权归我所有。你可以选择删除里面的文章替换上自己的博客文章，如需转载需要与我联系授权 **。



## 修改博客仓库名称

进到你自己的博客仓库，**修改博客仓库名称成你自己的用户名**。因为 github page 解析的时候找的是这个 username.github.io的仓库名，**这一步非常重要**。

![修改仓库名称](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

此时，不出意外的话，打开域名 https://username.github.io 就能看到你刚搭建的博客了。*注意替换 username成你自己的github 用户名*。

## 博客配置

上面点开域名看到的还是我的博客配置，显示的博客名字也是我的。还需要更改配置才是你的博客。

博客的配置文件是仓库根目录下的_config.yml文件，直接点开它编辑。

![config文件](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

你还需要更改以下配置：

### 博客名称和描述

![更改名称](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

分别是博客名称和描述，自己任意写点啥。

### 博客社交链接

![更改社交链接](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

这里配置社交链接按钮，没配的不显示，我现在配了知乎、邮箱、github账号三个。其他你想加自己加上就可以。

###  配置gitalk

这个是评论功能的配置。评论功能基于gitalk，在配置文件中找到gitalk配置项目：

修改规则如下：

```yml
gitalk:
  clientID: <你的clientID>
  clientSecret: <你的clientSecret>
  repo: <你的repository名称>
  owner: <你的GitHub用户名>
```

原理是利用github的issues评论文章。其中clientID和clientSecret需要[点击这里创建](https://github.com/settings/applications/new)

![创建gitalk鉴权app](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

点绿色按钮创建，成功之后会得到以上两个id，修改配置即可。

### Google站长统计

首先你要去注册一个[Google Analytics]( https://analytics.google.com/analytics/ )账号，它可以统计你博客网站的访问人数，访问来源等非常丰富的网站数据。如果你不在乎可以不用跳过这节。不过要把配置中我的`google_analytics: UA-XXXXXXX-X`删除，**否则统计到我的去了**。

```
# Enter your Google Analytics web tracking code (e.g. UA-2110908-2) to activate tracking
google_analytics: UA-XXXXXXX-X
```

下面是我的网站实时分析页面展示：

![google分析页面](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

由于不可描述的原因，国内注册账号可能会遇到问题，所有不配置也没关系。

### 博客网址配置

```
# Your website URL (e.g. http://barryclark.github.io or http://www.barryclark.co)
# Used for Sitemap.xml and your RSS feed
url: https://yourname.github.io
```

这里配置你自己的博客地址。

### 配置提交

对_config.ymld的修改需要提交才能生效，点下图中绿色按钮提交。

![配置提交](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

**done! 现在输入上面提到的博客地址，回车，你拥有了自己的博客。**



## 如何写博客

好了，博客有了。如何更新文章呢？

文章用markdown语法，写好统一放在_post文件夹下上传，git page会自动从你的git仓库拉去解析成网页，立刻就能在你的博客网页浏览。

关于文章的**命名格式**：博客文章必须按照统一的命名格式 `yyyy-mm-dd-blogName.md` 比如我这篇博客的名字是`2019-11-22-create_blog_with_github_pages.md`

**看到这里，如果只是简单的想写博客，后面的不看也可以了，你已经拥有了自己的博客！后面章节是记录一些DIY的过程。**

另外，发现最近用我这个模板的同学越来越多，如果搭建过程中有什么问题，可以在我的公众号「后端技术学堂」讨论交流。

![公众号二维码](https://upload-images.jianshu.io/upload_images/7842464-15f939ec039690f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 本地博客预览

到目前为止，我们提交的文章都是必须上传到github仓库才能预览。如果你想写完在本地浏览器看一下效果在上传也是可以的，因为不是所有人都有这样的需求。

###  安装 Ruby 和 DevKit

在官网下载，[点这里]( https://rubyinstaller.org/downloads/ )下载适合系统版本的 [Ruby+Devkit](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg) 包。安装，弹出的窗口选3

![安装ruby](https://c-ssl.duitang.com/uploads/blog/202009/07/20200907170405_047cf.thumb.1000_0.jpeg)

`gem -v` `ruby -v` 查看得到版本号就说明成功了。

如果是在墙内，需要切换安装源到https://gems.ruby-china.com/。墙外请忽略。

`gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/` 切换安装源

`gem sources -l` 查看版本

### bundler安装

`gem install bundler` 安装bundler 

`bundle -v 查看版本
 bundle config mirror.https://rubygems.org https://gems.ruby-china.com` 切换安装源

