---
title: 调试-VSCode
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-19 20:53:53
password:
summary:
tags: VSCode
categories: 调试
---

# VSCode调试

## 在nodejs中调试

1. 点击图标

![](Snipaste_2020-04-26_18-04-19.png)

2. 编辑或新建配置文件

![](Snipaste_2020-04-26_18-05-46.png)

3. 点击编辑的图标

​	进入编辑配置文件的文件（没有配置文件会新建）

4. 编辑配置文件

![](Snipaste_2020-04-26_18-09-49.png)

![](Snipaste_2020-04-26_18-12-20.png)

5. 进行调试

   单击`F5`进行调试，在代码的左侧`鼠标右键单击`添加断点

   ![](Snipaste_2020-04-26_18-15-20.png)

   ![](Snipaste_2020-04-26_18-14-39.png)

## 在Chrome中调试

1. 下载`Debugger for Chrome` 插件

![](Snipaste_2020-04-26_18-18-20.png)

2. 编辑配置文件

   ​	点击添加配置的时候，有两种选择

   ![](Snipaste_2020-04-26_18-23-11.png)

   >attach:
   >
   >​	会跟踪你已经运行的程序
   >
   >launch:
   >
   >​	会创建一个新的程序，并以一个新的用户打开新的窗口

![](Snipaste_2020-04-26_18-35-53.png)

3. 进行调试

   我已经在本地启动了项目

   ![](Snipaste_2020-04-26_18-38-21.png)

   我用`launch`模式进行调试，`F5`选择对应配置

   ![](Snipaste_2020-04-26_18-40-30.png)

   

可以进行调试：

![](Snipaste_2020-04-26_18-41-33.png)

## 留坑待填

​	我使用`attach` 模式去调试，但是失败了

![](Snipaste_2020-04-26_17-54-07.png)

报了一个这个错，试了很多方法还是不行



更多详细配置可以看官方文档：https://github.com/Microsoft/vscode-chrome-debug