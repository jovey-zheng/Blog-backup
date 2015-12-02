title: 使用Hexo + NexT搭建静态博客
date: 2015-09-18 15:06:37
categories:
  - 笔记随笔
tags:
  - Hexo
  - NexT
  - github
  - 博客
---
# 前言
[Github ](https://github.com/)为广大开发者提供了一个非常好的平台，不仅是代码的开源，同时[ Github ](https://github.com/)还提供了开发者可以在[ Github ](https://github.com/)上建立自己的站点（GithubPage）的一个非常有意思的功能。这个功能的局限是只能创建静态的网站，那么我们可以使用一些工具来快速创建这一网站。
本文旨在帮助刚接触[ Github ](https://github.com/)新手，想利用[ Github ](https://github.com/)来创建自己的站点、个人博客等。大神可以忽视__(:з」∠)__。

# 准备
你需要在[ Github ](https://github.com/)上创建一个属于自己的账户，然后新建一个仓库（`new repository`），并命名为 `YourSiteName.github.io/com`，此时[ Github ](https://github.com/)会帮助你初始化一个静态网页，你可以根据自己的喜好选择一些模版（~~这都不是重点~~），接着尝试访问下你所建的站点，成功后就可以开始动工了。

# 关于 Hexo
* **A fast, simple & powerful blog framework**
* **快速，简单而高效的静态博客框架**
 * **超快速度：** Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
 * **支持 Markdown：** Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。
 * **一键部署：** 只需一条指令即可部署到 GitHub Pages, Heroku 或其他网站。
 * **丰富的插件：** Hexo 拥有强大的插件系统，安装插件可以让 Hexo 支持 Jade, CoffeeScript。

# 关于NexT
![Theme-NexT](/blog/images/article_img/hexo-next.jpg)
* **NexT is built for easily use with elegant appearance. First things first, always keep things simple**
* **NexT 主旨在于简洁优雅且易于使用，所以首先要尽量确保 NexT 的简洁易用性。**

这是一个扩展主题，由[ iissnan ](https://github.com/iissnan)开发，`精于心，简于形`的理念。

<!-- more -->

# 正题
上面是对搭建博客的一些技术了解，接下来进入正题。

## Hexo 初始化博客框架
### 1. 安装 Hexo
Hexo 安装和搭建依赖[ Nodejs ](https://nodejs.org/en/)和[ Git ](http://git-scm.com/)，可自行下载。
```
$ npm install -g hexo-cli
```

### 2. 初始化框架
```
$ hexo init <yourFolder>
$ cd <yourFolder>
$ npm install
```
 初始化完成大概的目录：
```
.
├── _config.yml //网站的 配置 信息，您可以在此配置大部分的参数。
├── package.json
├── scaffolds 	//模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。
├── source 	//资源文件夹是存放用户资源的地方。
|   ├── _drafts
|   └── _posts
└── themes 	//主题 文件夹。Hexo 会根据主题来生成静态页面。
```

### 3. 新建文章（创建一个 Hello World）
```
$ hexo new "Hello World"
```
 在 `/source/_post` 里添加 `hello-world.md` 文件，之后新建的文章都将存放在此目录下。

### 4. 生成网站
```
$ hexo generate
```
 此时会将 `/source` 的 `.md` 文件生成到 `/public` 中，形成网站的静态文件。

### 5. 服务器
```
$ hexo server -p 3000
```
 输入 `http://localhost:3000` 即可查看网站。

### 6. 部署网站
```
$ hexo deploy
```
 部署网站之前需要生成静态文件，即可以用 `$ hexo generate -d` 直接生成并部署。此时需要在 `_config.yml` 中配置你所要部署的站点：
 ```
 ## Docs: http://hexo.io/docs/deployment.html
	deploy:
	  type: git
	  repo: git@github.com:YourRepository.git
	  branch: master
  ```
### 7. 更多
* 官网 - [[Hexo]](https://hexo.io/zh-cn/)
* 配置相关 - [[Hexo | 配置]](https://hexo.io/zh-cn/docs/configuration.html)
* 更多的命令 - [[Hexo | 指令]](https://hexo.io/zh-cn/docs/commands.html)

那么到此为止网站的雏形算是完成了，接下来你就要自己去管理和完善个人网站了。

## 使用 NexT 主题让站点更酷炫
### 1. 使用
```
$ cd your-hexo-site
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
 从Next的 `Gihub` 仓库中获取最新版本。

### 2. 启用
需要修改 `/root/_config.yml` 配置项 `theme`：
```
# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: next
```

### 3. 验证是否启用
```
$ hexo s --debug
```
 访问 `http://localhost:4000`，确保站点正确运行。（~~此命令可以做平时预览用~~）

### 4. 更多
* Next官网 - [[NexT]](http://theme-next.iissnan.com/)
* 主题设定 - [[NexT | 主题设定]](http://theme-next.iissnan.com/theme-settings.html)
* 第三方服务 - [[NexT | 第三方服务]](http://theme-next.iissnan.com/third-party-services.html)

启用 `NexT` 主题成功，那么你的网站变得酷炫（简约）。

# 最后
[我的博客](http://jovey-zheng.github.io/blog)
[NexT 官方实例](http://notes.iissnan.com/)

** 有任何疑问和建议可以留言，将在第一时间为你解答 **