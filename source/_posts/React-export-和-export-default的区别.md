---
title: React-export 和 export default的区别
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:05:42
password:
summary:
tags: 疑惑
categories: React
---

# export default 和 export 的区别

> export 用于导出常量、函数、文件、模块等。

## 区别

+ 通过export导出，导入时需要加{}，export defalut 导出的则不需要
+ export default为模块指定默认输出，不需要知道加载模块的变量名
+ 一个文件或模块里面，只能存在一个export default，import 和 export可以存在任意个。



