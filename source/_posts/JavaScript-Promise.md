---
title: JavaScript-Promise
top: false
cover: false
toc: true
mathjax: true
date: 2020-06-16 14:57:22
password:
summary:
tags: 基础
categories: JavaScript
---

# Promise

promise.resove()–> 产生一个promise对象

resolve()、reject()并不停止Promise，可使用`return resolve(...) 或 return reject(...)`

建议不要再`then()`方法里面定义Reject的回调函数，使用catch()

catch()返回的也是一个Promise对象，可以在之后使用`then()`

finally()的回调不接受任何参数，不依赖于Promise的执行结果，finally()相当于then的特例

all()接受一个数组或者具有Interator接口且成员是Promise的实例（如果不是Promise的实例，会调用Promise.resolve()方法，将参数转为Promise参数）的参数，当参数的所有状态变成resolve，Promise.all()的状态才变成resolve，返回值是每个成员返回的resolve()。当有一个参数的状态变成reject，Promise.all()就会变成reject，返回值是第一个被reject的实例的返回值。对于异常的捕捉如果成员有catch方法据不会传递到Promise.all().catch()。

rece()跟all()差不多，但是只要有一个状态发生变化，rece()的状态就会发生变化。

------

Promise.prototype.then()

Promise.prototype.catch()

Promise.prototype.finally()

Promise.prototype.all()

Promise.prototype.race()