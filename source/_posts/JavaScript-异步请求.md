---
title: JavaScript-异步请求
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-27 09:59:05
password:
summary: 
tags: 异步请求
categories: JavaScript
---

# JavaScript-异步请求

> 最近在写项目中看到发请求中使用了`fetch`，以前用的`axios`于是想知道这两种技术有什么区别，就写了这篇文章，打算以理论+实践的方式试一下这两者的区别

## ajax

## fetch

### fetch与ajax的区别

+ 当响应状态码是404或500时，fetch会将Promise的状态变成resolve，但是resolve返回值的ok属性设置为false，只要当网络故障或请求被拒绝的时候才会标记成reject
+ fetch可以接受跨域cookies，也可以使用fetch建立跨域会话
+ fetch不会发送cookies，除非使用credentials初始化选项

### 带请求参数

### 带凭证

### 上传JSON

### 上传文件

### 检测是否成功

### Request对象

### Header对象

### Response对象

### Body对象

## axios

