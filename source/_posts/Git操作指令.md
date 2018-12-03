---
title: Git操作指令
date: 2017-07-07 10:27:50
category: "Git" 
tags: "Git"
description: "分布式版本控制工具"
---

##  Git简介

### Git是什么？

Git是目前世界上最先进的分布式版本控制系统（没有之一）

***

## Git指令

### 创建Git版本库

要创建一个 git 版本库，首先我们创建一个叫 my-git 的文件夹，并进入

``` bash
$ mkdir my-git
$ cd my-git
```

使用 <font color="red">git init</font> 指令

``` bash
$ git init
```

这样，这个名叫 my-git 的文件夹就是我们的 git 版本库

### 把文件添加到版本库

首先我们创建一个叫 testGit 的 txt 文件，向其中随意写一些内容

第一步：使用 <font color="red">git add xxx</font> 指令，把文件添加到仓库，xxx 为文件的全名

``` bash
$ git add testGit.txt
```

第二步：<font color="red">git commit -m 'xxx'</font> 指令，把文件提交到仓库，'' 中的内容为对这篇 txt 内容的描述

``` bash
$ git commit -m 'my git test'
```

这样第一个版本就被提交到 git 仓库，修改文件内容，重复上面的两步，就会有第二个版本被提交到 git 仓库

使用 <font color="red">git diff</font> 指令，查看和上一个版本有什么不同

``` bash
$ git diff
```

### 查看当前仓库的状态

使用 <font color="red">git status</font> 指令，查看当前的仓库处于状态

``` bash
$ git status
```

状态可以为：

1.修改了修改了文件是否添加到仓库（Changes not staged for commit），即修改完没有执行任何指令
2.添加完毕是否提交的仓库（Changes to be committed），即修改完执行了 git add
3.没有任何修改（nothing to commit, working tree clean），即修改完执行了 git add 与 git commit

### 版本的回退

使用 <font color="red">git log</font> 指令，查看现在的仓库，都有哪些版本

``` bash
$ git log
```

使用 git log 输出的版本信息可能会有一点多，换作使用 <font color="red">git log --pretty=oneline</font> 指令，查看简略一点的仓库版本信息

``` bash
$ git log --pretty=oneline
```

输出的信息中会有一长串版本号，类似于（0732c5c74b09e92dbe4eaa00f4270899b43739bb），是 git 计算出的一个非常大的数字

使用 <font color="red">git reset</font> 指令，回退版本

git reset 后面可以跟三种类型，git reset [--soft | --mixed | --hard]

使用 <font color="red">git reset --hard HEAD^</font> 指令，回退到上一个版本

``` bash
$ git reset --hard HEAD^
```

也可以使用 <font color="red">git reset --hard xxx</font> 指令，回退到指定的版本，xxx为版本号

``` bash
$ git reset --hard 0732c5c74b09e92dbe4eaa00f4270899b43739bb
```

### 工作区 & 暂存区

工作区：就是这个文件夹里能看到的目录，工作区有一个隐藏目录 .git，这个不算工作区，而是 git 的版本库

暂存区：暂存区在 git 的版本库中，git add xxx 添加的文件进入暂存区，git commit 就是将暂存区的内容提交

如果想要撤销工作区的修改，可以使用<font color="red">git checkout -- xxx</font>，xxx 为要撤销的文件名字

``` bash
$ git checkout -- testGit.txt
```

如果想要撤销暂存区的文件，可以使用<font color="red">git reset HEAD xxx</font>，将文件放回工作区，xxx 为要撤销的文件名字

``` bash
$ git reset HEAD testGit.txt
```

### 删除文件

若想从版本库中删除一个文件：

使用 <font color="red">git rm xxx</font> 指令，删除一个一个文件，并且使用 <font color="red">git commit</font> 提交

``` bash
$ git rm testGit.txt
$ git commit -m 'my test git'
```

若错误删除了一个文件：

使用 <font color="red">git checkout -- xxx</font> 指令，恢复这个文件，git checkout 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”

``` bash
$ git checkout -- testGit.txt
```

### 远程仓库

比如说把我自己的 github 当作远程仓库，先配置 ssh

若添加到远程仓库：

先使用 <font color="red">git remote add origin xxx</font> 指令，xxx 为 github（远程仓库） 地址，在使用 <font color="red">git push</font> 命令，把当前分支 master 推送到远程仓库

``` bash
// origin 为远程仓库的名字，名字可以随意起
$ git remote add origin git@github.com:duanran1996112/duanran1996112.github.io.git

// 若是第一次提交需要加上 -u，合并关联 本地的 master 分支 和 远程仓库的 master 分支
// origin 为上一步起的远程仓库的名字
$ git push -u origin master

// 若不是第一次提交
$ git push origin master
```

若想从远程仓库克隆：

使用 <font color="red">git clone xxx</font> 指令，克隆一个本地库， xxx 为 github（远程仓库） 地址

``` bash
$ git clone git@github.com:duanran1996112/duanran1996112.github.io.git
```

### 分支系统

我们若想使用分支系统，合作修改项目，要经历以下的步骤：

首先，我们创建一个分支，使用 <font color="red">git branch xxx</font> 指令，xxx 为创建分支的名字

``` bash
// 创建一个叫 dev 的分支
$ git branch dev
```

若，只使用 <font color="red">git branch</font> 指令，还可以查看有哪些分支，正在工作的分支是哪一条

``` bash
$ git branch
```

其次，我们切换到这个分支，使用 <font color="red">git checkout xxx</font> 指令，xxx 为切换分支的名字

``` bash
// 切换到一个叫 dev 的分支
$ git checkout dev
```

上面这两步，可以合并为一步，使用 <font color="red">git checkout -b xxx</font> 指令

``` bash
$ git checkout -b dev
```

现在，我们在 dev 分支上，我们在这个分支上，修改 testGit 文件的内容并且提交

``` bash
$ git add testGit.txt
$ git commit -m 'test branch dev'
```

然后，我们使用 <font color="red">git checkout master</font> 指令，切换回 master 分支

``` bash
$ git checkout master
```

再然后，我们合并这两个分支，使用 <font color="red">git merge xxx</font> 指令，xxx 为我们刚才创建分支的名字

``` bash
$ git merge dev
```

我们也可以，不选择合并，而选择综合 两个 分支 的内容，创建一次新的提交（commit）。使用 <font color="red">git merge --no-ff -m " ..." xxx</font> 指令，引号中的 ... 为这次提交的描述，xxx 为刚才创建的分支

``` bash
$ git merge --no-ff -m "new commit" dev
```

最后，我们使用 <font color="red">git branch -d xxx</font> 指令，删除我们刚才创建的分支，xxx 为我们刚才创建的分支。

``` bash
$ git branch -d dev
```

若，我们创建的分支，在某些情况下，无法被删除。可以使用 <font color="red">git branch -D xxx</font> 指令，强制进行删除，xxx 为我们刚才创建的分支

``` bash
$ git branch -D dev
```

至此，一次分支的操作，全部进行完成