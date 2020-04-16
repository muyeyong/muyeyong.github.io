---
title: CSS-基础选择器
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:03:16
password:
summary:
tags: CSS基础
categories: CSS
---

# 基础选择器

> 分为标签选择器、id选择器、类选择器和通用符选择器

## 标签选择器

以html的标签名作为选择器，进行选择

```css
p {
	color: red;
}
```

## 类选择器

样式点定义 结构类调用 一个或多个  开发最常用

```css
.red {
    color: red;
}
```

+ 不能使用纯数字或中文作为类名
+ 类名名字过长用-分隔

可以给clas属性使用多个类名，只需使用空格分开。

+ 对于多个类，属性值重复的时候怎么解决？





## id选择器

样式#定义 结构id调用 只能调用一次  别人切勿使用

```css
<style>
	#pink {
		
	}
</style>
<div id="pink"></div>
```



## 通配符选择器

>\* 通配符，会修改所用标签的样式，不需要调用