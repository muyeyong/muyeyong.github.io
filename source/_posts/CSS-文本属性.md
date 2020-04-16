---
title: CSS-文本属性
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:03:58
password:
summary:
tags: CSS基础
categories: CSS
---

# CSS文本属性

> 可定义文本的外观，文本颜色、对齐文本、装饰文本、文本缩进、行间距等

## 文本颜色

> 可以通过color属性定义文本颜色

```css
div {
	color: red;
}
```

| 表示       | 属性值                   |
| ---------- | ------------------------ |
| 预定义颜色 | red green blue           |
| 十六进制   | #fff000 （开发用的最多） |
| RGB        | reg(255,255,255)         |

## 对齐文本

> text-align属性设置文本的水平对齐方式

| 属性值 | 说明                |
| ------ | ------------------- |
| left   | 左对齐,默认对齐方式 |
| center | 居中对齐            |
| right  | 右对齐              |

## 装饰文本

> text-decoration属性设置文本装饰

| 属性值       | 说明             |
| ------------ | ---------------- |
| none         | 默认值，没有装饰 |
| underline    | 下划线           |
| overline     | 上划线           |
| line-through | 删除线           |

## 文本缩进

> text-indent设置首行缩进

| 属性值      | 说明                           |
| ----------- | ------------------------------ |
| 像素点 20px | 缩进值                         |
| em ： 2em   | 相对当前文字大小距离，相对单位 |

## 行间距

> line-height 设置行间距离，设置行间距改变的是上下间距

![](image-20200331150959563.png)