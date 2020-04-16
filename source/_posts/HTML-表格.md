---
title: HTML-表格
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:01:40
password:
summary:
tags: HTML基础
categories: HTML
---

# 表格

&lt;table 属性：&gt; <br/>
 &lt;tr&gt;  &lt;th&gt; &nbsp; 表头 &nbsp;&lt;/th&gt; &lt;/tr&gt; <br/>
 &lt;tr&gt; &lt;td&gt; &nbsp; 普通单元格 &nbsp;&lt;/td&gt; &lt;/tr&gt; <br/>
&lt;/table&gt;

|   属性名    |             作用             |                           说明                           |
| :---------: | :--------------------------: | :------------------------------------------------------: |
|    width    |         设置表格的宽         | 跟height搭配使用时，只需要设置其中一个属性，另一个自适应 |
|   height    |         设置表格的高         |                           同上                           |
| cellspacing | 设置单元格与单元格之间的间隙 |                                                          |
|   border    |       设置表格边界宽度       |                                                          |
| cellpadding | 设置单元格内容与单元格的间隙 |                                                          |
|    align    | 设置表格相对于周围元素的位置 |                                                          |

## 表单区域

>thead 和 tbody, thead区别于th标签,thead表示的是表头区域，th表示表头单元格

## 合并单元格

>跨行合并: rowspan ='合并单元格的的个数'，操作的目标单元格是操作的最上的单元格
>
>跨列合并：colspan ='合并单元格的个数'，操作的目标单元格是操作的最左的单元格

### 合并单元格的步骤

1. 确定跨行还是跨列合并
2. 操作目标单元格
3. 删除多余的单元格

