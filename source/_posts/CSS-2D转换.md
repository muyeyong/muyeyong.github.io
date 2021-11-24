---
title: CSS-2D转换
top: false
cover: false
toc: true
mathjax: true
date: 2020-10-12 22:17:43
password:
summary: CSS动画-2D转换
tags: 2D转换
categories: CSS动画
---

# CSS-2D转换

transform(转换)可以实现元素的位移、旋转、缩放等效果，2D在二维坐标系中进行转换。

## translate（移动）

> 改变元素在页面中的位置，类似于定位

### 语法

```
transform: translate(x,y);
transform: translateX(n);
transform: translateY(n);
```

### 重点

* translate最大优点是：不会影响其他元素的位置，但可以覆盖到其他元素上面
* translate中的百分数是相对于自身元素的translate:(50%,50%)，相对自身长宽移动50%
* 对行内标签没有效果

![](1.gif)

## rotate （旋转）

> 让元素在2d平面内顺时针或逆时针旋转

### 语法

```
transform: rotate(度数)// 90deg
```

### 重点

* 度数为正顺时针旋转，度数为负逆时针旋转
* 默认旋转中心为元素的中心点

## scale（缩放）

> 放大或者缩小盒子

### 语法

```
transform: scale(x,y);
transform: scale(x);//相当于 scale(x,x)
```

## 重点

* x y之间用,隔开
* x y都是数字，代表放大倍数
* 不会影响其他盒子

对比，粉色的盒子使用了`scale`，红色的盒子使用的是改变宽高，会影响其他的盒子。

![](2.gif)

## 2D转换中心点

> 设置元素转换的中心点

### 语法

```
transform-origin: x y;
```

### 重点

* x y之间用空格隔开
* 默认的中心点是元素的中心点 transform-origin: 50% 50%;
* x y 可以是像素点 或 方位名词（top left right bottom cnnter）

```
 .rotate{
            width: 100px;
            height: 100px;
            transform-origin:left bottom ;
            margin: 100px auto;
            background: pink;
            transition: all 1s;

        }
.rotate:hover{
        transform: rotate(90deg);
        }
```

![](3.gif)

