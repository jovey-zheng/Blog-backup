title: NPM一些常用命令
date: 2015-09-17 15:57:11
categories:
  - npm
tags:
  - npm
  - module
---
# 关于NPM

NPM的全称是Node Package Manager，是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node模块（包）的标准。
就目前而言，NPM的官网[[1]](https://www.npmjs.com/)拥有18万的packages。国内的镜像是淘宝所提供的CNPM[[2]](http://npm.taobao.org/)，与NPM相同，它会每隔10分钟就同步一次。

# 一些常用命令
* `npm -v`: 查看npm安装的版本
* `npm init`: 引导你创建一个package.json文件，包括名称、版本、作者这些信息等
* `npm install <modulename>`: 安装模块
* `npm install <modulename> -g`: 安装全局模块
* `npm install <modulename>@1.0.0`: 安装指定版本的模块
* `npm install <modulename> -save`: 安装模块并添加到package.json依赖中

* `npm uninstall <modulename>`: 卸载模块
* `npm cache clean`: 清除缓存

* `npm help`: 查看帮助命令
* `npm ls`: 查看当前目录安装的依赖
* `npm ls -g`: 查看全局目录安装的依赖

<!-- more -->

* `npm view <modulename>`: 查看包的package.json
* `npm view <modulename> dependencies`: 查看包的依赖关系
* `npm view <modulename> repository.url`: 查看包的源文件地址

* `npm update <modulename>`: 更新模块
* `npm remove <modulename>`: 移除模块

# 题外话CNPM
有时候会出现NPM无法使用的情况，此时可以尝试使用CNPM解决此问题。
## 淘宝NPM镜像
这是一个完整 `npmjs.org` 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

## 使用
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
## 安装模块
与NPM类似，将`npm install <...>`改为`cpm install <...>`

# 更多
更多命令参考文档[[3]](https://docs.npmjs.com/)