---
title: CSS实战-快报
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-28 15:58:10
password:
summary:
tags: 快报
categories: CSS实战
---

# CSS实战-快报

实现效果：

![](Snipaste_2020-04-28_16-32-33.png)

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>快报</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    li {
      list-style: none;
    }

    /* a {
      text-decoration: none;
      color: #666;
    } */

    .box {
      width: 290px;
      height: 160px;
      margin: 100px auto;
      border: 1px solid #ccc
    }

    .box h3 {
      height: 30px;
      font-size: 14px;
      line-height: 30px;
      font-weight: 400;
      padding-left: 15px;
      border-bottom: 1px dotted #ccc;
    }

    .box ul li a {
      font-size: 12px;
      color: #666;
      text-decoration: none;
    }

    .box ul li {
      padding-left: 18px;
      padding-top: 10px;
      font-size: 12px;
    }

    .box ul li a:hover {
      text-decoration: underline;
    }
  </style>
</head>

<body>
  <div class="box">
    <h3>新闻大揭秘</h3>
    <ul>
      <li><a href="#">【惊天大新闻】我也不知道写点啥</a></li>
      <li><a href="#">【惊天大新闻】我也不知道写点啥</a></li>
      <li><a href="#">【惊天大新闻】我也不知道写点啥</a></li>
      <li><a href="#">【惊天大新闻】我也不知道写点啥</a></li>
    </ul>
  </div>
</body>

</html>
```

