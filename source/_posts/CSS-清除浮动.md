---
title: CSS-清除浮动
top: false
cover: false
toc: true
mathjax: true
date: 2020-05-07 09:09:12
password:
summary:
tags: CSS基础
categories: CSS
---

# CSS清除浮动

## 为什么需要清除浮动

​	当容器高度为`auto`的时候，且容器内包含浮动的元素，这种情况下容器不能自动拉伸适应内容的高度，使得内容溢出到容器外面，这种现象称为浮动溢出。

​	没有使用浮动

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>css为什么需要清除浮动</title>
  <style>
    .box {
      width: 500px;
      background-color: skyblue;
    }

    .div1 {
      height: 300px;
      width: 100px;
      background-color: pink;
    }

    .div2 {
      height: 400px;
      width: 200px;
      background-color: purple;

    }

    .footer {
      height: 100px;
      background-color: black;
    }
  </style>
</head>

<body>

  <div class="box">
    <div class="div1">1</div>
    <div class="div2">2</div>
  </div>
  <div class="footer">footer</div>

</body>

</html>
```



![](Snipaste_2020-05-07_13-51-47.png)

​	父盒子没有定义高度，子盒子定义了高度，父盒子可以自适应高度。

​	当使用浮动，父盒子不能自适应高度。

![](Snipaste_2020-05-07_14-00-04.png)

## 清除浮动的方法

+ **使用带clear属性的空元素（额外标签法）**

  > 在浮动元素后加一个空元素，例如`<div class='clear'></div>` ，并在css中`.clear{clear:both}`，就可以清除浮动。

  ![](Snipaste_2020-05-07_14-09-36.png)

  这种方法代码少，但添加了空白标签，代码不优雅，后期不容易维护。

+ **给父元素添加overflow属性**

  > 给父元素添加`overflow:hidden` 或 `overfloaw:auto`，可以清除浮动。

  ![](Snipaste_2020-05-07_14-18-14.png)

+ **使用:after伪元素**

  > 在父级元素的最后添加一个`:after`伪元素

​	在父元素添加类似如下的样式

```
.clearfix:after {
    content: '.';
    height: 0;
    display: block;
    clear: both;
}
//兼容IE6、7
.clearfix {
    *zoom: 1; 
} 
```

![](Snipaste_2020-05-07_14-39-43.png)

​	伪元素的`display`是`block` ，它实际上是一个不可见的块级元素，这是第一种方法的变形使用。

​	需要兼容IE6、7.

+ **使用双伪元素**

  > 完全闭合浮动

  ​	父元素添加类似的样式

  ```
  .clearfix:before, .clearfix:after {
          content: ""; 
          display: table;
      }
      .clearfix:after {
          clear: both;
      }
      //兼容IE6、7
      .clearfix {
          *zoom: 1;
      }
  ```

  

  

  