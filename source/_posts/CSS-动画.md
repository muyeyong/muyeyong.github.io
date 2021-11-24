---
title: CSS-动画
top: false
cover: false
toc: true
mathjax: true
date: 2020-10-15 22:04:27
password:
summary: 动起来，更精彩！
tags: 动画
categories: CSS
---

# CSS-动画

## 动画的基本使用

1. 定义动画

```
//keyframes定义动画
@keyframes 动画名称{
	0%{
		width:100px;
	}
	100%{ 
		width:50px;
	}
}
0% 是动画的开始，可以使用from代替
100% 是动画的结束，可以使用to代替
0% 100% 代表的是一个动画序列
```

2. 调用动画

```
div{
	width: 100px;
	hieght: 50px;
	background: pink;
	animation-name: 动画名称;
	animation-duration: 动画持续时间;
}
```

### 动画序列

* 可以有多个动画序列
* 里面的百分数必须是整数
* 百分比是总时间的百分比

```
@keyframes 动画名称{
	0%{
	
	}
	20%{
	
	}
	30%{
	
	}
	.....
	100%{
	
	}
}
```

## 动画的常见属性

| 属性                       | 描述                                                         |
| -------------------------- | ------------------------------------------------------------ |
| @keyframes                 | 规定动画                                                     |
| animation                  | 所有动画属性的简写属性，除animation-play-state               |
| animation-name             | 规定@keyframes动画的名称（必须）                             |
| animation-duration         | 规定动画完成一个周期所花费的时间（秒/毫秒，必须），默认是0   |
| animation-time-function    | 规定动画的运动曲线，默认是'ease'                             |
| animation-delay            | 规定动画何时开始，默认是0                                    |
| animation-interation-count | 规定动画播放的次数，默认是1，infinite：无限循环              |
| animation-direction        | 规定动画是否在下周期逆向播放，默认是'normal'，'alternate'：逆向播放 |
| animation-play-state       | 规定动画是否正在运行或暂停。默认是'running'，还有'pause'     |
| animation-fill-mode        | 规定动画结束后的状态，forwards：保持最后的状态，backwards：回到初始状态 |

## 动画属性简写

```
animation: 动画名称 持续时间 运动曲线 何时开始(动画延时) 播放次数 是否反方向 动画起始或结束后的状态
```

## 速度曲线

| 值          | 描述                                         |
| ----------- | -------------------------------------------- |
| linear      | 匀速                                         |
| ease        | 默认。动画以低俗开始，然后加速，在结束前变慢 |
| ease-in     | 动画以低速开始                               |
| ease-out    | 动画以低速结束                               |
| ease-in-out | 动画以低速开始和结束                         |
| steps()     | 指定了时间函数中的间隔数量（步长）           |

