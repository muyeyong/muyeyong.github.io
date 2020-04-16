---
title: CSS-盒子模型
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:03:30
password:
summary:
tags: CSS基础
categories: CSS
---

# css盒子模型

> 盒子模型包含四个部分：内容(centent)、内边距(padding)、边框(border)和外边距(margin)

![](Snipaste_2020-04-15_08-44-07.png)

## 边框(border)

| 属性         | 说明                                                      |
| ------------ | --------------------------------------------------------- |
| border-style | 设置边框的样式，常用 dotted:点线 dashed：虚线 solid：实线 |
| border-width | 设置边框的宽度，一般用像素点设置                          |
| border-color | 设置边框的颜色                                            |

![](Snipaste_2020-04-15_10-16-34.png)

表格有自己的边框，单元格也有自己的边框，如果两个叠加在一起会造成边框比较宽，可以使用`border-collapse`设置表格行和单元格的行是否合并。

| 属性            | 属性值                                                       |
| --------------- | ------------------------------------------------------------ |
| border-collapse | **separate**：边框独立(标准html)  **collapse**：相邻边框被合并 |

+ 每条边框可以分别设置

  > 通过指定top left buttom right 设置单个边框的样式

  ![](Snipaste_2020-04-15_10-29-20.png)

+ border的复合写法

  > border: color style size,顺序没有严格要求

  ### 注意

  **border会影响盒子实际大小**

![](Snipaste_2020-04-15_10-31-47.png)

像这种带边框的盒子，边框大小会影响盒子实际大小

解决方案：

   	1. 量盒子宽度的时候不要把边框算进去
            	2. 对盒子大小进行调整，减去边框的宽度

## 内边距(padding)

> 内边距设置的是内容与边框之间的距离