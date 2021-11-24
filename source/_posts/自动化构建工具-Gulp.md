---
title: 自动化构建工具-Gulp
top: false
cover: false
toc: true
mathjax: true
date: 2020-06-01 14:20:24
password:
summary:
tags: Gulp
categories: 自动化构建工具
---

# 自动化构建工具-Gulp

> 在Javascript的开发过程中，经常会遇到一些重复性的任务，比如合并文件、压缩代码、检查语法错误、将Sass代码转成CSS代码等等。通常，我们需要使用不同的工具，来完成不同的任务，既重复劳动又非常耗时。grunt，gulp都是为了解决这个问题而发明的工具，可以帮助我们自动管理和运行各种任务，很多人认为，在操作上，它要比Grunt简单。



## task

> 异步的 javascript 函数，可以接收一个回调函数作为参数

### 组合任务

> series() 和 parallel()



## 处理文件

> 暴露 `src()` 和 `dest()` 方法处理计算机存放的文件

`sec()`: 接收glob参数，读取文件，产生流，产生的流因该从任务中返回并发出异步完成的信号

`stream`: 提供的主要API `.pipe()` ，用于连接准换流 或 可写流

`dest()`: 接收输出目录作为参数，并且产生一个Node流，通常作为终止流



## Glob

> glob 是由普通字符和/或通配字符组成的字符串，用于匹配文件路径。可以利用一个或多个 glob 在文件系统中定位文件。

### 字符串片段 与 分隔符

字符串片段（segment）是指两个分隔符之间的所有字符组成的字符串。在 glob 中，分隔符永远是 `/` 字符 - 不区分操作系统 - 即便是在采用 `\\` 作为分隔符的 Windows 操作系统中。在 glob 中，`\\` 字符被保留作为转义符使用。



### 特殊字符： *

在一个字符串片段中匹配任意数量的字符，包括零个匹配。对于匹配单级目录下的文件很有用。

下面这个 glob 能够匹配类似 `index.js` 的文件，但是不能匹配类似 `scripts/index.js` 或 `scripts/nested/index.js` 的文件。

```js
'*.js'
```

### 特殊字符： **

在多个字符串片段中匹配任意数量的字符，包括零个匹配。 对于匹配嵌套目录下的文件很有用。请确保适当地限制带有两个星号的 glob 的使用，以避免匹配大量不必要的目录。

下面这个 glob 被适当地限制在 `scripts/` 目录下。它将匹配类似 `scripts/index.js`、`scripts/nested/index.js` 和 `scripts/nested/twice/index.js` 的文件。

```js
'scripts/**/*.js'
```

### 特殊字符： !(取反)

由于 glob 匹配时是按照每个 glob 在数组中的位置依次进行匹配操作的，所以 glob 数组中的取反（negative）glob 必须跟在一个非取反（non-negative）的 glob 后面。第一个 glob 匹配到一组匹配项，然后后面的取反 glob 删除这些匹配项中的一部分。如果取反 glob 只是由普通字符组成的字符串，则执行效率是最高的。



## 使用插件

 node流 ？ 转换流？

## 文件监控

> gulp api 中的 `watch()` 方法利用文件系统的监控程序（file system watcher）将 [globs](https://www.gulpjs.com.cn/docs/getting-started/explaining-globs) 与 [任务（task）](https://www.gulpjs.com.cn/docs/getting-started/creating-tasks) 进行关联。它对匹配 glob 的文件进行监控，如果有文件被修改了就执行关联的任务（task）。如果被执行的任务（task）没有触发 [异步完成](https://www.gulpjs.com.cn/docs/getting-started/async-completion) 信号，它将永远不会再次运行了。

