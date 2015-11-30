title: 更新Github的Fork代码
date: 2015-10-06 16:46:12
categories:
  - 笔记随笔
tags:
  - Git
  - 项目管理
---
[github](https://github.com/) 提供了一个非常便捷的功能叫 **Fork**，即用户可以很方便的从别的仓库中复制一份代码到自己的名下。但是有一个不足是 [github](https://github.com/) 并不提供自动更新功能，那么此时就需要我们自己手动更新这个 **Fork** 仓库代码。

1. 安装 [github客户端](https://desktop.github.com/) 或者 [Git](http://www.git-scm.com/download/)。
2. **clone** 需要更新的 **Fork** 分支到本地：
 - github客户端：直接打开客户端，添加 **Fork** 分支，然后** clone**。
 - Git命令：
  ```
  $ git clone git@github.com:yourname/repos.git <yourfolder>
  ```
 **注**：github客户端 **clone** 成功后，可以使用 **git bash** 进行命令行输入。

3. 将源分支添加到该仓库的远程分支中：
 ```
 $ git remote add author git@github.com:author/repos.git
 ```
 此时可以使用 `$ git remote -v` 查看远程分支列表，结果如下：
 ```
 author git@github.com:author/repos.git (fetch)
 author git@github.com:author/repos.git (push)
 origin  git@github.com:yourname/repos.git (fetch)
 origin  git@github.com:yourname/repos.git (push)
 ```

4. **fetch** 源仓库代码的最新版本到本地：
```
$ git fetch author   //这里的`author`是上面从源分支添加到远程分支的分支名
```

5. 合并两个版本的代码：
```
$ git merge author/master
```

6. 将本地代码更新到 **Fork** 仓库：
```
$ git push origin master
```

# 其他方法
重复上述的1-3，然后使用 `git pull author master` 把源仓库的最新代码拉下来，然后使用第6步的方法，将代码推到自己的 fork 的仓库中。