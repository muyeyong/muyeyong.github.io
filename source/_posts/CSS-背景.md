---
title: CSS-背景
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:02:53
password:
summary:
tags: CSS基础
categories: CSS
---

# css背景

> 主要分为背景颜色、背景图片、背景颜色的重复以及背景颜色的定位

## background-color

> 默认是transparent（透明），可以用16进制、RGB以及直接设置颜色来设置

```
p { background-color: silver }
div { background-color: rgb(223,71,177) } 
body { background-color: #98AB6F }
pre { background-color: transparent; } 

```

## background-image

> 设置背景图片，使用背景图片是为了更好的控制图片的位置（相对于imag标签），使用url导入

```
code { background-image: url("comet.jpg"); }
blockquote { background-image: url("c:\InetPub\MyPixs\comet.jpg"); } 
br { background-image: url(http://Fred.com/ImageFile/Q.gif); } 
body { background-image: none; 
```

### 设置背景颜色透明度

```
background-color: rgba(255, 0, 0,0.3)// a 指的是alpha（透明度），取值在0~1之间
```



## backgroung-repeat

> 设置背景图片的平铺方法，默认为repeat

| 参数      | 说明             |
| --------- | ---------------- |
| repeat    | 图像在x、y轴平铺 |
| no-repeat | 图像不平铺       |
| repeat-x  | 图像延x轴平铺    |
| repeat-y  | 图像延y轴平铺    |

```
menu { background: url("images/aardvark.gif"); background-repeat: repeat-y; } 
p { background: url("images/aardvark.gif"); background-repeat: no-repeat; } 
```

## background-position

> 设置背景图片的位置，可以使用具体数值排列、方位排列和混合排列

### 具体数字排列

> 具体数值：百分数 | 由浮点数字和单位标识符组成的长度值

```background-position:20px 40px;
background-position:20px 40px;
background-position:20px;
```

如果指定两个数值，第一个是x轴，第二个是y轴

如果只指定一个数值，默认指定为x轴，y轴默认居中

![](Snipaste_2020-04-13_09-41-27.png)

## 方位排列

> 使用top,buttom,left,right,center来设置

```
background-position: top right;
background-position: top;
```

如果只指定一个，默认指定为x轴，y轴默认为center

## 混合排列

> 结合数值排列和方位排列

```
background-position:20px top;
```

第一个是x轴数值，第二个是y轴数值，如果指定的是不合法的定位，图片会定位为默认状态（x:0px , y:0px）



##  background-attachment

> 设置背景图片的滚动和固定

| 属性值 | 说明           |
| ------ | -------------- |
| scroll | 背景图片可滚动 |
| fixed  | 背景图片固定   |



