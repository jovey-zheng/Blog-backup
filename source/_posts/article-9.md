title: 使用 Git 管理项目 - reset 与 rebase
date: 2015-11-04 17:49:09
categories:
  - Git
tags:
 - git
 - 项目管理
 - reset
 - rebase
---
在日常管理项目的过程中，可能会遇到提交的 commit/merge 并不是想要的，或是操作失误提交了，那么此时我们需要把不想要的 commit/merge 取消掉，如果做到呢？Git 为我们提供了一个 `reset` command，很好地解决了这个问题。

# reset
## 1. 命令说明
```
$ git reset [--hard|soft|mixed|merge|keep] [<commit>|<HEAD>]
```
常用的是`[--hard|soft|mixed]`，本文主要使用`--hard`作为例子进行说明。

## 2. 本地仓库
在本地仓库执行 `$ git reset --hard HEAD^` 可以将本地的仓库回滚到上一次提交时的状态，`HEAD^`指的是上一次提交。

同时你也可以执行 `$ git reset --hard fc232ae` 将其回滚到 `fc232ae` commit 时的状态。

## 3. 远程仓库
以上操作只会对本地仓库造成影响，而远程仓库的源码和 commit 信息并不会因此改变。那么此时我们需要另外一个 command 来改变远程仓库的状态。

**注意，此时不要在上一步的操作之后执行`$ git pull` ，因为这个操作会使本地仓库的状态与远程同步。**
```
$ git push origin [branch] -f
```
执行此命令后，Git 会将远程仓库的状态与本地仓库的保持一致，即回滚状态。

<!-- more -->

在更新代码时，难免一次到位，此时就会生成许许多多的 commit 。比如同一个文件，反复地修改代码，反复地提交，此时会有5，6个 commit 甚至更多，那么你会在提交 list 中看到一大串的 commit 记录，会觉得很是头疼，杂乱。此时我们需要把这些 commit 整合以下，合并到一个 commit 中，其他的 commit 都 squash 到第一 commit 中，那么就需要用到 `rebase`。
# rebase
## 1. 说明
```
$ git rebase -i [branch|<commit>]
```

你可以直接进入某个分支的 rebase 也可以进入某次 commit 的 rebase，如果你是项将某些 commit 合并，那么建议使用 `$ git rebase -i <commit>`。

此外 rebase 还提供三个操作命令，分别是 `--continue`、`--absort` 和 `--skip`，这三个命令的意思分别是“继续”、“退出”和“跳过”。
## 2. 查看记录
```
$ git log
```
执行此命令即可看到当前分支下所有的提交记录，然后根据个人需要复制其中的 commit 的 SHA 进行 rebase 操作。
## 3. rebase
执行：
```
$ git rebase -i 9cbc329
```
然后就会看到：
```
pick fb554f5 This is commit 1
pick 2bd1903 This is commit 2
pick d987ebf This is commit 3
# Rebase 9cbc329..d987ebf onto 9cbc329
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
```
那么其中 `pick fb554f5 This is commit 1`我们可以把它分成三部分去解释：
- `pick`:：操作，即 rebase command
- `fb554f5`：commit shortID，提交的简写ID
- `This is commit 1`： commit message，提交时填写的提交信息

此时我们可以看到输出结果中所提供的一些操作方法，比如 `pick`、`squash`、`edit` 等。那么重要的是 `pick` 和 `squash`。

接着我们需要把 `2bd1903` 和 `d987ebf ` 合并到 `fb554f5 ` 中，做如下操作（**注意：此时是 VIM 的操作界面，熟悉 Linux 的同学可以无视，不熟悉的可以[简略的指导](#简略的指导)**）：
```
pick fb554f5 This is commit 1
squash 2bd1903 This is commit 2
squash d987ebf This is commit 3
# Rebase 9cbc329..d987ebf onto 9cbc329
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
```
在做完以上修改操作后输入 `:x` 保存文件并退出界面，然后就会看到：
```
$ git rebase -i 9cbc329
rebase in progress; onto 9cbc329
You are currently rebasing branch 'master' on '9cbc329'.

nothing to commit, working directory clean
The previous cherry-pick is now empty, possibly due to conflict resolution.
If you wish to commit it anyway, use:

    git commit --allow-empty

Otherwise, please use 'git reset'
Could not apply 9cbc329f722f8e531496da70ee3857b031574b6d... squash commit on rebase
```
此时用 `$ git status`  查看会看到：
```
$ git status
rebase in progress; onto 9cbc329
You are currently rebasing branch 'master' on '9cbc329'.
  (all conflicts fixed: run "git rebase --continue")

nothing to commit, working directory clean
```
紧接着我们需要执行 `$ git rebase --continue` 操作：
```
$ git rebase --continue
[detached HEAD 2bd1903...d987ebf] squash commit on rebase
 Date: Tue Nov 3 10:09:43 2015 +0800
 1 file changed, 149 insertions(+), 154 deletions(-)
 rewrite test.js (72%)
Successfully rebased and updated refs/heads/master.
```
最后我们需要把修改合并好的 commit push 到远程仓库上：
```
$ git push origin [branch] -f
```
到此为止，整个 rebase 操作都已完成。
你会看到类似：
![before](/blog/images/article_img/4.png)
变成类似：
![after](/blog/images/article_img/5.png)

# 简略的指导
在 VIM 的操作界面下，需要按 `I/Insert` 键进行插入修改文本操作，修改完文本之后需要按 `Esc` 键退出编辑状态，然后输入 `:q` 是离开，输入 `:!q` 是强制离开，输入 `:x` 是保存修改并离开。
在 rebase 修改文本结束后需要输入 `:x` 进行保存。