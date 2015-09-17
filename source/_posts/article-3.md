title: 使用Git管理项目-初始化
date: 2015.09.07 15:57:59
categories:
	- Git
tags:
	- git
	- 项目管理
---
# 获取Git仓库
获取Git仓库的方式主要分为两种。第一种是在现有项目或目录下导入所有文件到 Git 中；第二种是从一个服务器克隆一个现有的 Git 仓库。

## GIT INIT
首先在现有的项目目录下输入：
> $ git init

这样就简单将你的目录转变成一个Git仓库。该命令将创建一个名为 `.git`的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。

如果你是在一个已经存在文件的文件夹（而不是空文件夹）中初始化 Git 仓库来进行版本控制的话，你应该开始跟踪这些文件并提交。
> $ git add .  //`.`是将目录的所有文件都添加（不包括空文件夹）
$ git commit -m 'first commit'  //提交修改（`'first commit'`为注释信息）

此时可以使用`$ git status`来查看当前文件状态。
我们还可以将项目加入到远程Git仓库：
>$ git remote add origin git@github.com:jovey-zheng/test.git  //加入到远程的Git仓库
$ git push -u origin master  //将项目推到Git仓库

这样我们就可以实现项目的远程操作。

<!-- more -->

## GIT CLONE
当远程Git仓库已经存在一个项目时，需要对此项目进行操作；或者有一个你想为此贡献自己一份力的开源项目时，就需要用到`$ git clone`。当你执行`$ git clone`命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。

克隆仓库的命令格式为`$ git clone [url]`。例如，要克隆 Git 的可链接库 test，可以用下面的命令：
> $ git clone git@github.com:jovey-zheng/test.git

这会在当前目录下创建一个名为 `test` 的目录，并初始化一个`.git`文件夹。
如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以使用如下命令：
> $ git clone git@github.com:jovey-zheng/test.git myTest

这将执行与上一个命令相同的操作，不过在本地创建的仓库名字变为 myTest。

Git 支持多种数据传输协议。上面的例子使用的是`SSH`传输协议，当然也可以使用`https://`协议。

关于SSH：《[SSH key生成](http://www.jianshu.com/p/697fe0815689)》
推荐：《[Git pro](http://git-scm.com/book/zh/v2)》