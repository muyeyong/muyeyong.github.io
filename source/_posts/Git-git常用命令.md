---
title: Git-git常用命令
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:06:51
password:
summary:
tags: 常用操作
categories: Git
---

# git常用命令

> 由于每次使用git命令的时候都要去google，特意写文章记录下，按照一个空白项目的开发流程来记录。

## 从远程仓库克隆

>  git clone  远程仓库的地址

![](1.png)

## 建立本地跟远程仓库的连接

> git remote add origin 远程仓库的地址
>
> > 其中remote 是指远程仓库，origin是给远程仓库添加别名



## 建立本地分支跟远程分支的关联

> git branch --set-upstream-to 远程分支名 本地分支名

## 将工作区的更改移到暂存区

> git add  [ 需要移动的范围]

+ `git add .` 将所有文件移到暂存区
+ `git add <filename1> <filename2> ... ` 将指定文件移到暂存区
+ `git add [dir1] [dit2] .. ` 将指定文件夹移到暂存区

## 将暂存区的更改提交到本地仓库

> git commit -m '对于本次提交的留言'

## 撤销上次提交

>git commit --amend

## 将本地仓库分支推送到对应的远程仓库的分支

> git push 远程仓库的名字 远程仓库分支名

## 将本地仓库分支推送到远程仓库的某分支

> git push 远程仓库名 本地分支名：远程分支名

