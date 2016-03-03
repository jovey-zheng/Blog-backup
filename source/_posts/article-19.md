title: Hexo 构建博客 - NexT 主题浅谈
date: 2016-03-03 14:16:35
categories:
  - 笔记随笔
tags:
  - Hexo
  - NexT
  - github
  - 博客
---

> 利用 [Github](https://github.com/) 所提供的 **Github Page** 去构建静态的网站已经变得越来越流行，如果还不了解怎么入门，可以阅读我之前的一篇文章[「使用Hexo + Next搭建静态博客」](http://www.jianshu.com/p/f66103553c45)。当然构建博客的方法不是只有一种，你也可以尝试其他方法，而本文主要是针对 **Hexo** 去叙述的。

可能看过[「使用Hexo + Next搭建静态博客」](http://www.jianshu.com/p/f66103553c45)这篇文章的同学都已经构建好了属于自己的博客了，那么接下来要说的就是关于 [NexT 主题](http://theme-next.iissnan.com/)中遇到的一些问题和提示。

# 关于 RSS
很多同学在看到别人的博客时，都会发现有订阅的功能（即 RSS），但无奈官方介绍比较少，所以无从下手。

那么下面将教大家如何去做：

1. 准备
你需要安装一个 **Hexo 插件**：
```
$ npm install --save hexo-generator-feed
```

2. 配置
接下来需要在 `_config.yml` 中配置一下，在 root 目录下的 `_config.yml` 中添加：
```
# Extensions
## Plugins: http://hexo.io/plugins/
plugins:
  hexo-generate-feed
```
然后在主题文件夹的 `_config.yml` 中配置：
```
# Set rss to false to disable feed link.
# Leave rss as empty to use site's feed link.
# Set rss to specific value if you have burned your feed already.
rss: /atom.xml
```

<!-- more -->

3. 生成 RSS Feed
配置完之后在 CLI 中运行：
```
$ hexo g
```
重新生成一次，你会在 `./public` 文件夹中看到 `atom.xml` 文件。然后启动服务器查看是否有效，之后再部署到 `Github` 中。

最后你可以看到：
![效果](/blog/images/article_img/19-1.png)

# 修改文件后不生效
> 有时候会发现，明明修改了文件的代码，然而没有生效。

其实不是没有生效，而是静态文件没有更新，此时你需要执行：
```
$ hexo clean
```
然后执行：
```
$hexo g
```
重新生成一次即可。

# 社交链接图标
说明一下，这些图标都是出自 [`FontAwesome - 4.4.0`](https://fortawesome.github.io/Font-Awesome/)，所以你可以根据自己的需求去修改图标。

实际效果：
![效果](/blog/images/article_img/19-2.png)

有的同学会发现自己的图标是个地球：
![效果](/blog/images/article_img/19-3.png)

需要配置的是主题文件夹下的 `_config.yml`，**注意：命名需要一致，包括大小写**：
```
# Social links
social:
  GitHub: https://github.com/XXX
  Twitter: https://twitter.com/XXX
  Weibo: http://weibo.com/XXX
  Facebook: https://www.facebook.com/XXX
  JianShu: http://www.jianshu.com/XXX

# Social Icons
social_icons:
  enable: true
  # Icon Mappings
  GitHub: github
  Twitter: twitter
  Weibo: weibo
  Facebook: facebook-square
  JianShu: heartbeat
```

# 阅读次数
简单介绍一下此功能的做法。

1. 准备
需要在 [LeanCloud](https://leancloud.cn) 申请一个帐号，进行一番配置后拿到 AppID 和 AppKey。

2. 配置
然后在主题文件夹下的 `_config.yml` 中配置：
```
# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
leancloud_visitors:
  enable: true
  app_id: XXX
  app_key: XXX
```

具体可以阅读[这篇文章](http://notes.xiamo.tk/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html)，里面的介绍非常详细！

# 最后
> 文章将持续更新，有任何疑问和建议可以在下面评论。
