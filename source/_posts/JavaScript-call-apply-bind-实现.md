---
title: JavaScript-call apply bind 实现
top: false
cover: false
toc: true
mathjax: true
date: 2020-05-26 14:33:45
password:
summary:
tags: 基础
categories: JavaScript
---

# call apply bind的实现

> call、apply和bind的主要目的是改变this的指向。call 和 apply的区别是接收参数的不同，call接收的一系列的参数，apply接收的是一组参数。而bind的与call和apply的区别是，bind返回一个函数，可以延迟执行，call和apply会立即执行。

## call的实现

```
// call 
// 改变 this 的指向，返回执行函数的结果
// 1: 判断上下文是否合法
// 2：绑定传入上下文
// 3：传递参数
// 4：返回执行结果


Function.prototype.myCall = function (context, ...args) {
  if (context === null || context === undefined) context = window;
  else context = Object(context);
  let sym = Symbol();
  context[sym] = this; //this指的就是调用 mycall的函数
  let result = context[sym](...args);
  delete context[sym];
  return result;
}

function saySomething(name) {
  console.log(this.str, name)
}

A = {
  str: '上帝说我帅的很'
}

saySomething.myCall(A, '花大姐')
```

![](Snipaste_2020-05-26_14-45-31.png)

## apply的实现

```
Function.prototype.myapply = function (context, arg) {
  if (context === null || context === undefined) context = window;
  else context = Object(context);
  let sym = Symbol();
  context[sym] = this;
  let result;
  function isLikeArrayOrArray(o) {
    if (Object.prototype.toString.call(o) === '[object Array]') return true;
    if (o && typeof o === 'object'
      && isFinite(o.lenght) && o.lenght >= 0
      && o.lenght === Math.floor(o.lenght)
      && o.lenght < Math.pow(2, 32)) return true;
    else return false;
  }
  if (arg) {
    if (isLikeArrayOrArray(arg)) {
      result = context[sym](...arg);
    } else {
      throw Error('参数类型错误');
    }
  } else {
    result = context[sym]();
  };
  delete context[sym];
  return result;
}


function printArray() {
  this.arr.forEach(item => console.log(item))
  if (arguments) {
    console.log(arguments)
  }
}

B = {
  arr: [1, 2, 3, 4, 5, 6, 7]
}

printArray.myapply(B, [])
```

![](Snipaste_2020-05-26_15-00-29.png)

## bind的实现

```
//bind   柯里化

Function.prototype.mybind = function (context, ...args1) {
  if (context === null || context === undefined) context = window;
  else context = Object(context);
  return (...args2) => this.call(context, ...args1, ...args2);
}

C = {
  name: '马大哈'
}

function sayMyName() {
  console.log('我是多余的参数...', arguments)
  console.log('My name is ' + this.name);
}

let hi = sayMyName.mybind(C, '1234,走起')

hi('456,出发')
```

![](Snipaste_2020-05-26_15-01-22.png)