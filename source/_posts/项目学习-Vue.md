---
title: 项目学习-Vue
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-02 09:44:36
password:
summary:
tags:
categories:
---

1.  ul li可以相互包含![](项目学习-Vue/Snipaste_2020-11-02_15-52-52.png)

2. 组件

   名字：例如组件的引入名字是： MycomponentName，在模板中: <my-component-name></my-component-name>

   ![](项目学习-Vue/Snipaste_2020-11-02_15-54-45.png)

   slot：对于一个通用的组件，但在每个对其引用的组件其表现形式有所不同，可以使用slot

   ​					![](项目学习-Vue/Snipaste_2020-11-02_15-55-41.png)

   传参：![](项目学习-Vue/Snipaste_2020-11-02_15-58-33.png)
   
   动态组件：
   
   ​	使用<keep-alive></keep-alive>缓存被创建的组件，组件要有自己的名字。

3. 插件
   + Swiper：滑动
   + better-scroll：滚动

4. 代码书写风格

   ![](项目学习-Vue/Snipaste_2020-11-03_10-22-10.png)

5. 用户体验

   * 数据没加载出来的过渡效果

     使用transition封装

![](https://i.loli.net/2020/11/05/fn1UlzWpjtboxV3.png)

# 我的不足

* 路由的使用（路由与组件解耦，语法）
* Vue过渡效果 （https://cn.vuejs.org/v2/guide/transitions.html）

