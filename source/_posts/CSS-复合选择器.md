---
title: CSS-复合选择器
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:03:06
password:
summary:
tags: CSS基础
categories: CSS
---

# css复合选择器

## 后代选择器

> 选择父选择器的的所有符合条件的子选择器，父选择器和子选择器可以是任意的基础选着器

```javascript
父选择器 子选择器 {
    设置属性
}
```

![](Snipaste_2020-04-09_17-15-05.png)

## 子元素选择器

> 选择直接关联的子选择器

```javascript
父选择器 子选择器{
    设置属性
}
```

![](Snipaste_2020-04-09_18-01-39.png)



## 并集选择器

> 实现多个选择起的集体声明

![](Snipaste_2020-04-09_18-11-37.png)

## 伪类选择器

> 伪类选择器通过`:`声明

### 链接伪类选择器

| 名称      | 效果             |
| --------- | ---------------- |
| a:hove    | 鼠标放上去触发   |
| a:active  | 点击链接时触发   |
| a:link    | 未访问链接       |
| a:visited | 链接被点击时触发 |

![](Snipaste_2020-04-12_10-36-44.png)

注意使用链接伪类的顺序： link => visited => hover => active

使用这种顺序的原因是为了防止前面触发的效果被后面出发的效果覆盖。

未点击链接前，link伪类长期处于激活状态，鼠标悬停（或点击）时，<a>链接同时处于link和hover(或active)状态，由于它们特指度相同，在同时激活的情况下，后出现的伪类样式会覆盖前面的伪类样式，故link状态必须写在hover(或active)之前。

再讨论hover和active的顺序，若把hover放在active后面，当点击链接一瞬，实际你在激活active状态的同时触发了hover伪类,hover在后面覆盖了active的颜色，所以无法看到active的颜色。故hover在active之前

其次，若把visited放在hover后面，那已访问过的链接一直触发着visited伪类，会覆盖hover样式。

最后，其实link、visited两个伪类之间顺序无所谓。（因为它俩不可能同时触发，即又未访问同时又已访问。）



## focus伪类

> 元素获取焦点时，添加样式，一般用于input表单

![](Snipaste_2020-04-12_10-44-19.png)