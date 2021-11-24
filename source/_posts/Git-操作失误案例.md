---
title: Git-操作失误案例
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-16 19:56:47
password:
summary:
tags: 失误案例
categories: Git
---

# Git操作失误案例

## git init 后找不到本地分支

![](Snipaste_2020-04-16_19-58-54.png)

  进行 `git init` 命令后，出现了master分支，然后我就傻傻的去查看本地分支`git branch -a`,但是没有任何输出：

![](Snipaste_2020-04-16_20-02-17.png)

  就觉得很奇怪，后面查资料才知道，**git 分支必须指向一个commit，没有任何commit就没有任何分支** ，提交（git commit过一次，以后新建的分支不管有没有提交数据，都会显示。



## 添加远程仓库后，看不到远程分支

   新建的本地仓库需要关联远程仓库，使用命令`git remote add origin git@github.com:muyeyong/React.git` 后，使用`git branch -a`查看所有分支（包括远程），发现只查到本地分支

![](Snipaste_2020-04-16_20-17-36.png)

  只是建立了连接，并没有获取远程分支的信息，还需要使用`git fetch `获取远程分支的所有信息。

![](Snipaste_2020-04-16_20-33-58.png)

 ### 补充：

 	git fetch 和 git pull 的不同：

​		首先使用一张盗的图

![](Snipaste_2020-04-16_20-32-56.png)

解释一下远程仓库副本： 本地的远程仓库缓存

git fetch 获取远程仓库，并没有合并到本地分支，一般情况下`git pull = git fetch + git merge`。

