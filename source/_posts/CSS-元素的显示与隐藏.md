---
title: CSS-元素的显示与隐藏
top: false
cover: false
toc: true
mathjax: true
date: 2020-08-09 15:27:05
password:
summary: 没错，我就是六娃
tags: 元素的显示与隐藏
categories: CSS
---

# CSS-元素的显示与隐藏

## dispaly

+ display：none; 隐藏对象
+ display：block；出了转换成块级元素外，同时还有显示元素的意思（inner-block是不是也可以？）

**diaplay隐藏元素后，不再占有原来的位置**

![](/Snipaste_2020-08-10_09-22-50.png)

![](Snipaste_2020-08-10_09-27-43.png)]()

## visibility

| 可选参数 | 说明                     |
| -------- | ------------------------ |
| inherit  | 继承上一个父元素的可见性 |
| visible  | 对象可见                 |
| hidden   | 对象隐藏                 |
| collapse | 主要用于隐藏表格的行或列 |

**visibility隐藏元素后，继续占有原来的位置**

### 默认为`visible` :

![](Snipaste_2020-08-10_09-31-02.png)

### 使用`hidden`，继续保留原来的位置：

![](/Snipaste_2020-08-10_09-32-21.png)

## overflow

对溢出的位置进行显示隐藏

| 可选参数 | 说明                         |
| -------- | ---------------------------- |
| visible  | 不剪切内容也不添加滚动条     |
| auto     | 在需要时剪切内容并添加滚动条 |
| hidden   | 不显示超过对象尺寸的内容     |
| scroll   | 总是显示滚动条               |

### `visible`，即默认情况

![](Snipaste_2020-08-10_09-38-07.png)

### `hidden`

![](Snipaste_2020-08-10_09-40-51.png)

### `scroll`

![](Snipaste_2020-08-10_09-48-21.png)