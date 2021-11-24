---
title: CSS-3D转换
top: false
cover: false
toc: true
mathjax: true
date: 2020-10-21 14:55:48
password:
summary: CSS-3D转换
tags: 3D转换
categories: CSS动画
---

# CSS-3D转换

## 特点

近大远小，可以被遮挡

## 三维坐标系

x轴：水平向右，向右为正值，向左为负值

y轴：垂直向下，向下为正值，向上为负值

z轴：垂直屏幕，屏幕向外为正，屏幕向内为负

## 3D移动 translate3d

> 3D移动在2D移动的基础上多了一个Z轴

### 语法

```
//分开写
transform: translateX(100px)
transform: translateY(100px)
transform: translateZ(1oopx)// translateZ一般用px作为单位
//合写
transform: translate3d(x,y,z)//x,y,z不能省略
```

![](1.gif)

除了位置变化外，其他并没有什么变化，是因为我们没有加透视，就像看3D电影没戴3D眼镜一样

## 透视 perspective

* 在2d平面内产生近大远小的视觉立体，但效果实在二维上呈现的，可以想象成3d物体投影在2d平面内

* 透视也成为视距：眼镜到屏幕的距离
* 透视的单位是像素、
* 透视写在被观察盒子的父盒子上面

![](2.gif)

加上透视后，可以观察到盒子在变大

