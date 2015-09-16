title: 使用Git管理项目-起步
date: 2015.09.07 12:06:21
categories:
- Git
tags:
- git
- 项目管理
---
# 关于版本控制
提到版本控制，那么我会想到的是SVN以及这里要说的Git。那什么是版本控制呢？版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

# Git基础-三种状态
Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。

![工作目录、暂存区域和Git仓库](http://upload-images.jianshu.io/upload_images/741039-4ff4205ccbd9519b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作“索引”，不过一般说法还是叫暂存区域。

基本的 Git 工作流程如下：
```
1.在工作目录中修改文件。

2.暂存文件，将文件的快照放入暂存区域。

3.提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。
```
如果 Git 目录中保存着的特定版本文件，就属于已提交状态。 如果作了修改并已放入暂存区域，就属于已暂存状态。 如果自上次取出后，作了修改但还没有放到暂存区域，就是已修改状态。

# 如何安装Git

> **Mac**: brew install git

> **Linux(Debian)** : apt-get install git-core

> **Linux(Fedora)** : yum install git-core

> **Windows** : 下载安装 [Git](http://git-scm.com)

配置
```
$ git config --global user.name "your name"
```
```
$ git config --global user.email "youremail@email.com"
```
使用 `--global` 可以使该命令只执行一次。

你可以通过如下的命令来查看你的配置信息：
```
$ git config --list

user.email=joveyzheng@qq.com

user.name=joveyzheng

color.status=auto

...
```
你可以通过输入 `$ git config <key> ` 来查看某一项的配置
```
$ git config user.name

joveyzheng
```
# 获取帮助
```
$ git help
```


文中多处借鉴《Git pro》，想获得更多了解推荐阅读： [Git Pro](http://git-scm.com/book/zh/v2)