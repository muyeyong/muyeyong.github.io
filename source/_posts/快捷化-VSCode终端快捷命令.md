---
title: 快捷化-VSCode终端快捷命令
top: false
cover: false
toc: true
mathjax: true
date: 2020-05-08 10:07:53
password:
summary:
tags: VSCode终端快捷命令
categories: 快捷化
---

# VSCode终端快捷命令

## 快速调出终端Tab 

> 当终端Tab切换到其它Tab时，终端Tab会消失，可以使用`Ctrl + ' `   快速调出终端Tab

![](1.gif)

## 终端清屏

> 使用nodejs调试的时候，输出会在终端显示。终端显示的东西越来越多，看起来很乱，是时候清理一下终端了。

+ 清除历史输出

  `Ctrl + L` ，在代码还在运行的使用这个命令是没有用的。

  ​	代码停止运行了

  ![](2.gif)

+ 清除全部输出

  `Ctrl + K` ，这个命令需要配置才能使用，在代码运行的时候也是可以使用的。

  1. 打开配置文件

     `Ctrl + Shift + P`  打开查询界面，输入 `open keyboard Shortcuts`

     ![](Snipaste_2020-05-08_10-42-09.png)

  2. 添加配置

  ```
    {
      "key": "ctrl+k",
      "command": "workbench.action.terminal.clear",
      "when": "terminalFocus"
    }
  ```

  ​	保存就可以使用`Ctrl + K `，干干净净的终端就来了。

  ![](3.gif)

