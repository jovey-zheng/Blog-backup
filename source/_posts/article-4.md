title: Brunch:快捷的HTML5构建工具
date: 2015.09.08 14:48:25
categories:
- 笔记随笔
tags:
- Brunch
- 构建工具
---
![Brunch](/blog/images/article_img/2.png)
# 了解Brunch（官方介绍）
* 编译你的脚本，模板，样式，链接它们
* 将脚本和模板封装进common.js/AMD模块里，链接脚本和样式
* 为链接文件生成源地图，复制资源和静态文件
* 通过缩减代码和优化图片来收缩输出，看管你的文件更改
* 并且通过控制台和系统提示通知你错误

# 安装
当你已经拥有Nodejs时（若没有，请到[nodejs官网](http://nodejs.org)下载），就可以直接使用`npm`运行：
> $ npm install -g brunch

# 新建 new
* 新建一个Brunch
> $ brunch new <skeleton-URL> [optional-output-dir]

`new`可以简写为`n`。
`<skeleton-URL>`指定一个架构，则项目会应用此架构进行初始化。
`[optional-output-dir]`指定输出目录，项目名称可以自定义。

例如：
> $ brunch n https://github.com/scotch/angular-brunch-seed myProject
08 Sep 12:20:32 - log: Cloning git repo "https://github.com/scotch/angular-brunc                                                                       h-seed" to "E:\myProject"...
08 Sep 12:20:49 - log: Created skeleton directory layout
08 Sep 12:20:49 - log: Installing packages...

当不指定输出文件夹时，必须保证放置新项目的文件夹为空，否则会`clone`失败。
<!-- more -->

# 构建 build
> $ brunch build --production

也可以简写为：
> $ brunch b -P

这样构建一个分布式项目，使得项目的体积变小。

# 使用 watch
> $ brunch watch --server

也可以简写为：
> $ brunch w -s

让Brunch看管你的项目，然后你就可以运行项目了。输出结果如：
>08 Sep 12:32:58 - error: { [Error: Component must have "E:\w\bower_components\console-polyfill\bower.json"] code: 'NO_BOWER_JSON' }
08 Sep 12:32:59 - info: application started on http://localhost:3333/
08 Sep 12:33:00 - warn: 'test\karma-e2e.conf.js' compiled, but not written. Check your javascripts.joinTo config.

# 注意
* **必要安装项**
Git安装：[Git](http://git-scm.com)
bower安装：
> $ npm install -g bower

* **运行项目前**
执行`$ bower install`对项目初始化。

# 参考
[【官网】](http://brunch.io/)[【README】](https://github.com/brunch/brunch/tree/master/docs)[【Command】](https://github.com/brunch/brunch/blob/master/docs/commands.md)
