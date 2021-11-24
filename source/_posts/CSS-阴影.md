---
title: CSS-阴影
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-26 19:31:37
password:
summary:
tags: CSS基础
categories: CSS
---

# 阴影

## 盒子阴影

> box-shadow设置盒子的阴影

![](yinying3.gif)

| 值       | 属性值                                             | 说明                   |
| -------- | -------------------------------------------------- | ---------------------- |
| h-shadow | 长度值                                             | 必需，水平阴影的位置   |
| v-shadow | 长度值                                             | 必需，垂直阴影的位置   |
| blur     | 长度值                                             | 可选，设置阴影的模糊度 |
| color    | 颜色值                                             | 可选，设置阴影的颜色   |
| inset    | 阴影在边框外，即阴影向外扩散,指定inset阴影在盒子内 | 可选                   |

## 水平、垂直阴影位置解释

```
 .nav {
      height: 300px;
      width: 300px;
      line-height: 300px;
      margin: 100px auto;
      background-color: pink;
      text-align: center;
      box-shadow: 10px 20px black;
    }
```



![](yinying1.gif)

![](Snipaste_2020-04-27_11-36-53.png)

### blur 属性解释

```
.nav {
      height: 300px;
      width: 300px;
      line-height: 300px;
      margin: 100px auto;
      background-color: pink;
      text-align: center;
      box-shadow: 5px 5px 5px rgba(0, 0, 0, .6);
    }
```

![](yinying2.gif)

`blur`越大越模糊，越小越清晰

### 关于`inset` 

​	使用 `inset` 关键字会使得阴影落在盒子内部，这样看起来就像是内容被压低了。 此时阴影会在边框之内 (即使是透明边框）、背景之上、内容之下。

![](Snipaste_2020-04-27_13-52-17.png)

## 文字阴影

> text-shadow，设置文字阴影，可以有两三个`lenght`参数

| 取值               | 说明                                   |
| ------------------ | -------------------------------------- |
| color              | 设置阴影颜色                           |
| offset-x  offset-y | 以文字中心为中标原点，设置`xy`轴的偏移 |
| blur               | 阴影模糊度                             |

```
 span {
      text-shadow: pink 10px 10px 10px;
    }
```



![](Snipaste_2020-04-27_13-51-10.png)