---
title: mongoDB异步取数据
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-29 13:58:09
password:
summary:
tags: mongoDB
categories: 数据库
---

# mongoDB异步获取数据

>介绍一下背景
>
>![](Snipaste_2020-04-29_17-57-16.png)
>
>​	这四个订单分类下面有若干订单，我需要统计出来可视化展示，我先获取了分类的唯一标识`_id`，把它们放入到一个数组中，接下来就是要获取每个分类下面有多少订单数目

​	接下来就是我的骚操作了：

​		数据库操作的代码：

![](image-20200429192823808.png)

​		请求过程：

![](Snipaste_2020-04-29_19-34-40.png)

![](Snipaste_2020-04-29_19-35-57.png)

​	首先就算分类下面没有订单，也会返回0，然后我们看数据库那一块代码。

![](Snipaste_2020-04-29_19-40-01.png)

​	有两个输出，从结果来看，查询还没有结束就已经返回了。是因为异步的原因，那么怎么解决等数据库查询结束后在返回我们需要的数据。



我的解决方案是使用`Async` ，首先需要安装`npm i anync --save`，查看文档

```
官方文档：https://github.com/caolan/async
第三方注释文档：https://github.com/bsspirit/async_demo
```

选择合适的api，针对我的使用场景，我选用了 `async.eachSeries`，一个一个按循序执行。

​	改造完的代码：

![](Snipaste_2020-04-29_19-52-50.png)

现在就没问题了：

![](Snipaste_2020-04-29_19-53-50.png)

还有其它的解决办法，我后续补充............................

