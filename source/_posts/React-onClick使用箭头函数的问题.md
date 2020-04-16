---
title: React-onClick使用箭头函数的问题
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:07:25
password:
summary:
tags: 疑惑
categories: React
---

# Reac中onClick使用箭头函数的疑问

如果直接写：

```
onClock = {this.handleClick(i)}
```

这样的话，在render的时候就已经执行了，肯定不行。



写成这样：

```
onClick= {this.handleClick}
```

需要携带参数过去的话就不好解决。



就需要匿名函数将参数带过去：

```
onClick = {()=> handleClick(i)}
```

闭包让```i``` 保持对renderSquare的 `i` 的引用。

