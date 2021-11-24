---
title: CSS实战-小米商品展示
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-24 16:50:56
password:
summary:
tags: padding margin 应用
categories: CSS实战
---

# CSS实战-小米商品展示

## 效果图

![](Snipaste_2020-04-24_17-14-40.png)

## 使用技能

​	1. PS的基本操作（测量视图的大小、取色）

​	2. margin、padding的使用

	3. 居中操作（块级元素居中、行内元素居中、图片居中）
 	4. 字体属性的应用
 	5. 行内元素 和 块级元素 的转换

## 操作

1. 先把图片截下来（选取效果图的第一个图片进行操作）

   使用`Snipaste` 截取图片

2. 分析构造

   ![](Snipaste_2020-04-24_17-19-10.png)

可以分为四个盒子

 		第一个盒子： 使用了图片，图片需要水平居中

​		二、三、四个盒子：都是对文字的操作

​		整体： 鼠标放上去会变成手，需要加上a链接，需要去除a链接的默认样式

3. 编码



## 代码

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>小米商品展示</title>
  <style>
    * {
      padding: 0;
      margin: 0;
    }

    a {
      text-decoration: none;
      color: #333;
    }

    body {
      background-color: #f5f5f5;
    }


    .product-father {
      display: block;
      width: 235px;
      height: 300px;
      margin: 100px auto;
      background-color: #fff;
    }

    .product {
      padding: 20px 0;
      height: 260px;
    }

    .product .figure-img {
      margin: 0 37px 18px 37px;
    }

    .product .figure-img img {
      width: 160px;
      height: 160px;

    }

    .product h4 {
      font-size: 14px;
      font-weight: 400;
      text-align: center;
      margin-bottom: 6px
    }

    .product .decs {
      font-size: 12px;
      text-align: center;
      color: #c2b0b0;
      padding-bottom: 17px;
    }

    .product .buttom {
      text-align: center;
      font-size: 14px;
    }

    .product .buttom .left {
      color: #ff6700;
    }

    .product .buttom .right {
      color: #c2b0b0;
      margin-left: 8px;
      text-decoration: line-through;

    }
  </style>
</head>

<body>
  <!-- shift + alt + 鼠标左键 -->
  <a href="#" class="product-father">
    <div class="product">
      <div class="figure-img">
        <img src="./imags/earphone.jpg" alt="耳机">
      </div>

      <h4>小米真无线蓝牙耳机Air 2</h4>
      <div class="decs">智能真无线，轻松舒适戴</div>
      <div class="buttom">
        <span class='left'>369</span> <span class="right">399</span>
      </div>
    </div>
  </a>

</body>

</html>
```

## 总结

+ 首先需要先消除每个元素的内外边距
+ 如果没有指定元素的宽或高，margin padding  不会撑大盒子
+ 注意HTML的语义化