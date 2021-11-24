---
title: CSS-无限循环
top: false
cover: false
toc: true
mathjax: true
date: 2020-09-17 09:09:24
password:
summary: 无限循环的奥义
tags: 动画效果
categories: CSS
---

# CSS-无限循环的奥义

先上图：

![](1.gif)

​	看起来是有点卡顿，但实际效果比gif图好那么一丢丢（不会出现上面的卡顿，在safari上面的效果如丝般润滑，上图是在chrome上的效果）。这是最近甲方爸爸需要实现的一个效果，作为一个前端小菜鸡，我顿时就蒙了，我还没做过这种动画效果，但是年轻人怕啥，上百度就完事。

## 需求分析

右边的动画效果分为三成，最上面的就是实际的数据展示，下面两层两个假的背景图需要实现不同速度的无限循环，显得~~好看点~~。盒子在移动的过程中需要做缩小的效果。

![](Snipaste_2020-09-17_09-55-34.png)

## 实现 

### 先动起来

使盒子或图片实现无限循环的效果，需要准备两份一模一样的dom，当第一个dom移动的时候第二个dom随着移动，第一个dom完成动画效果后，**直接替换第二个dom**，也就是所有的动画效果只是在第一个dom上作用。要实现实现动画效果就需要`animation`属性。

HTML结构

```
<div class="father">
	<div class="son1"></div>
	<div class="son2"></div>
</div>
```

将需要动画css效果加到`father`上，它就会控制下面的`son1`、`son2`实现无限循环，`son1`、`son2`的结构是一模一样的，但是要给`son2`添加`left：-100%`是为了让`s1`、`s2`隔开，然后就是给`father`添加css效果。

```
 getScrollStyle(offsetHeight, speed = 20,type){
        const uid = Math.random().toString(36).substr(2);
        const style = document.createElement('style');
        style.type = 'text/css';
        if(type === scrollType.rb){
            style.innerHTML = `@-webkit-keyframes rowup${uid} {
              0% {
                  -webkit-transform: translate3d(-1%, 0, 0);
                  transform: translate3d(-1%, 0, 0);
              }
             
              100% {
                  -webkit-transform: translate3d( 100%,0, 0);
                  transform: translate3d(100%,0, 0);
              }
          }
          @keyframes rowup${uid} {
              0% {
                  -webkit-transform: translate3d(-1%, 0, 0);
                  transform: translate3d(-1%, 0, 0);
              }
              100% {
                  -webkit-transform: translate3d(50%,0,  0);
                  transform: translate3d(100%,0,  0);
              }
          }
          .rowup-${uid}{
              -webkit-animation: ${Math.floor(offsetHeight*1000 / speed)}ms rowup${uid} linear infinite normal;
              animation: ${Math.floor(offsetHeight*1000 / speed)}ms rowup${uid} linear infinite normal;
          }`
          this.animationTime = Math.floor(offsetHeight*1000 / speed)
          this.animationTime = (this.animationTime/1000).toFixed(3)
        }else if(type === scrollType.vb){
              style.innerHTML = `@-webkit-keyframes rowup${uid} {
              0% {
                  -webkit-transform: translate3d(-1%, 0, 0);
                  transform: translate3d(-1%, 0, 0);
                   opacity: 0;
              }
             
              100% {
                  -webkit-transform: translate3d( 100%,0, 0);
                  transform: translate3d(100%,0, 0);
                   opacity: 1;
              }
          }
          @keyframes rowup${uid} {
              0% {
                  -webkit-transform: translate3d(-1%, 0, 0);
                  transform: translate3d(-1%, 0, 0);
                   opacity: .4;
              }
              90%{
                 opacity: .8;
              }
              100% {
                  -webkit-transform: translate3d(100%,0,  0);
                  transform: translate3d(100%,0,  0);
                   opacity: .4;
                 
              }
          }
          .rowup-${uid}{
              -webkit-animation: ${Math.floor(offsetHeight*1000 / speed)}ms rowup${uid} linear infinite normal;
              animation: ${Math.floor(offsetHeight*1000 / speed)}ms rowup${uid} linear infinite normal;
          }`
        }else if(type === scrollType.bi){
              style.innerHTML = `@-webkit-keyframes rowup${uid} {
              0% {
                  -webkit-transform: translate3d(-1%, 0, 0);
                  transform: translate3d(-1%, 0, 0);
              }
              100% {
                  -webkit-transform: translate3d( 100%,0, 0);
                  transform: translate3d(100%,0, 0);
              }
          }
          @keyframes rowup${uid} {
              0% {
                  -webkit-transform: translate3d(-1%, 0, 0);
                  transform: translate3d(-1%, 0, 0);
              }
             
              100% {
              
                  -webkit-transform: translate3d(100%,0,  0);
                  transform: translate3d(100%,0,  0);
              }
          }
          .rowup-${uid}{
              -webkit-animation: ${Math.floor(offsetHeight*1000 / speed)}ms rowup${uid} linear infinite normal;
              animation: ${Math.floor(offsetHeight*1000 / speed)}ms rowup${uid} linear infinite normal;
          }`
        }
        
       document.getElementsByTagName('head')[0].appendChild(style);
       return `rowup-${uid}`;
    },
//记得给father添加这个
.father{
	overflow: hidden;
}

```

其中**offsetHeight**是`father`的，**type**是根据不同效果层返回不同的动画效果，将返回的css添加到`father`上就可以实现循环滚动。

### 缩小实现

使用`keyframes`（上一个效果已经用了）

```
 @keyframes firstScaleDraw{
           0%{
                 transform: scale(1); 
            }
            100%{
              top: 36%;
              transform: scale(0); 
            }
  }
//将关键帧添加到需要添加的dom上
 .s1 .market_size, 
 .s1  .market_share, 
 .s1  .patent_number,
 .s1 .difficulty,
 .s1 .market_maturity, 
 .s1 .financing_scale,
 .s1 .ave_invt_size,
 .s1 .conversion_rate{
   -webkit-animation-name: firstScaleDraw; /*关键帧名称*/
   -webkit-animation-timing-function: ease-in-out; /*动画的速度曲线*/
   -webkit-animation-iteration-count: infinite;  /*动画播放的次数*/
}
//还没添加动画时间
```

#### 需要注意的

上面的动画效果还没有添加时间，我本来是给它单独添加时间的，但是注意到我们已经实现了一个动画效果，如果两个动画效果时间不一致会造成以下效果。

![](2.gif)

所以你需要这样：

![](Snipaste_2020-09-17_10-22-54.png)

![](Snipaste_2020-09-17_10-22-59.png)

统一他们的动画时间。

### 关于背景

如果你用两张一模一样的图片去循环会造成背景太过于单调，如果使用不同的图片，在两个dom进行切换的时候会造成卡顿的效果，体验不要，这时候就需要美工的帮助了🤣，切一个长图出来，这里也需要注意一点就是背景图片的效果一定要设置的比较宽，不要使用`width:100%`，一定要比`100%`宽，因为你的图是一个长图。

## 想暂停的话

老爹说过要用魔法对抗魔法，用css对抗css。

当鼠标悬停的时候，需要停止动画效果，我最开始的时候使用js来控制的，监听hover事件，但效果一言难尽。最后发现只需要添加一个属性就好。

```
animation-play-state:paused;
```

要控制多个动画效果的话，还是监听dom的hover事件，控制变量，动态给需要的dom添加上面的属性。

## 踩过的坑

### 跳帧

背景图片在移动的过程中，其实有透明度的变化，本来设置从0% - 100%直接变化，但是注意到第二个dom其实是没有动画效果的，就会造成跳帧的不好体验，所以0%处不能设置成 `opacity:0`，需要自己慢慢调试设置对应的值，我设置的值为`opacity:0.4`，还有就是0%和100%的效果需要保持一致。不然也会造成跳帧。

![](Snipaste_2020-09-17_10-50-00.png)

## 想要不同方向的移动

设置这个属性就好

```
translate3d(100%,0,  0);
```

## 参考文章

[1](https://www.xiabingbao.com/css3/2017/07/03/css3-infinite-scroll.html)

[2](https://segmentfault.com/a/1190000021821797)

