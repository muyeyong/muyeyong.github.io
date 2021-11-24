---
title: JavaScript-宏任务、微任务 和 EventLoop
top: false
cover: false
toc: true
mathjax: true
date: 2020-06-16 14:35:36
password:
summary:
tags: 基础
categories: JavaScript
---

# 宏任务、微任务 和 EventLoop

## 让我们来讲一个小故事

 6月份在火辣辣的长沙，走在热浪滔天的五一广场，口干舌燥，想来一杯冰凉凉的奶茶，但买奶茶的人特别多，作为一个文明人，我当然选择排队。前面有一个火辣辣的美眉，她点了一杯波霸珍珠奶茶，然后到取餐区等奶茶制作完，但她突然想加一点料，就跟店员说，加一份芋圆，店员为了响应她的要求，不得不延迟对后面排队人的服务，这时美眉又说加一份葡萄干，服务员又进行响应，延迟+2，经过若干这样的交流（延迟+n），美眉回到了取餐区了。~~我突然感慨，奶茶真好看~~。

 在这个故事中，假设奶茶店只有两名员工，一名制作奶茶，一名接待客户。对于接待客户的那名员工来说，每个客户的点单过程可以看成是一个`同步任务`（可以想象成可以立马完成），但等待奶茶制作时间相对较多，客户需要到取餐区等待，可以将它看成一个`异步任务（宏任务）`。对于客户提出的其它要求，就像上面的加料，服务员不能说让她到后面去排队，而是满足她的要求，这些要求是在进行`异步任务（宏任务）`的时候加进去的，可以将它看成`微任务`，没有完成当前客户的 `微任务`之前，不会处理下一位客户的需求 ，而`微任务`会在`宏任务`之前执行，就像加料（`微任务`）会在整杯奶茶制作（`宏任务`）完之前完成。对于后面的每一个客户都会这样处理，就形成了`Event Loop`。

 **总结一下：**当执行一段代码的时候，先执行同步代码块，当遇到`微任务`就把它放到`微任务队列`中去，当遇到`宏任务`就放到`宏任务队列`里面去，当同步代码块执行完毕后，就去检查`微任务队列`，执行完全部微任务，当微任务执行完后，就去检查`宏任务队列`，执行全部宏任务，这就是一轮`Event Loop`。

 **注意：** 在执行`宏任务`的时候可以添加`微任务`，毕竟我也喜欢加料。

![](cde525c2-e5e6-49fa-9387-a8d470677056.png)

 **插播一下其他人的理解：**

> 一个掘金的老哥（ssssyoki）的文章摘要： 那么如此看来我给的答案还是对的。但是js异步有一个机制，就是遇到宏任务，先执行宏任务，将宏任务放入eventqueue，然后在执行微任务，将微任务放入eventqueue最骚的是，这两个queue不是一个queue。当你往外拿的时候先从微任务里拿这个回掉函数，然后再从宏任务的queue上拿宏任务的回掉函数。 我当时看到这我就服了还有这种骚操作。

## 什么是宏任务

 macrotask，也叫 tasks，主要的工作如下

- 创建主文档对象,解析HTML,执行主线或者全局的javascript的代码,更改url以及各种事件。
- 页面加载,输入，网络事件，定时器。从浏览器角度看，宏任务是一个个离散的，独立的工作单元。
- 运行完成后，浏览器可以继续其他调度，重新渲染页面的UI或者去执行垃圾回收

一些异步任务的回调会以此进入 `macrotask queue(宏任务队列)`，等等后续被调用，这些异步函数包括：

- setTimeout
- setInterval
- setImmediate (Node)
- requestAnimationFrame (浏览器)
- I/O
- UI rendering (浏览器)

## 什么是微任务

 microtask，也叫 jobs，主要的工作如下

- 微任务是更小的任务，微任务更新应用程序的状态，但是必须在浏览器任务继续执行其他任务之前执行，浏览器任务包括重新渲染页面的UI。
- 微任务包括Promise的回调函数，DOM发生变化等，微任务需要尽可能快地，通过异步方式执行，同时不能产生全新的微任务。
- 微任务能使得我们能够在重新渲染UI之前执行指定的行为，避免不必要的UI重绘，UI重绘会使得应用状态不连续

另一些异步回调会进入 `microtask queue(微任务队列)` ，等待后续被调用，这些异步函数包括：

- process.nextTick (Node)
- Promise.then()
- catch
- finally
- Object.observe
- MutationObserver

> 这里有一点需要注意的：`Promise.then()` 与 `new Promise(() => {}).then()` 是不同的，前面的是一个微任务，后面的 `new Promise()` 这一部分是一个构造函数，这是一个同步任务，后面的 `.then()` 才是一个微任务，这一点是非常重要的。

## 什么是Event Loop

Event Loop 是一个数据结构，用于等待和发送消息和事件，在不同的地方有不同的实现。

## 来上代码

### 在浏览器中的表现

- 示例一

  ```
  setTimeout( () => console.log(4))
  new Promise(resolve => {
    resolve()
    console.log(1)
  }).then( () => {
    console.log(3)
  })
  Promise.resolve(5).then(() => console.log(5))
  console.log(2)
  ```

   我们直接在浏览器里面运行这段代码：

  ![](Snipaste_2020-06-17_11-47-04.png)

  分析一下代码的运行：

  第一轮`event loop`，整体代码（script）作为一个宏任务

   **执行同步代码，注册宏任务、微任务**

   `setTimeout` 是异步代码，注册宏任务，放入宏任务队列

   `new promise(...)`构造函数是同步代码，输出 1，`.then()`，注册微任务，放入微任务队列

   `Promise.resolve().then(...)`，注册微任务，放入微任务队列

   `console.log()`同步代码输出 2

   **执行微任务**

   执行`new Promise().then(...)`的微任务，输出 3

   执行`Promise.resolve().then(...)`的微任务，输出 5

   微任务全部执行完成，第一轮 `event loop`完成

  第二轮`event loop`，取出宏任务队列的宏任务

   执行`setTimeout(...)`，输出 4

- 示例二

  在线示例：https://codesandbox.io/s/interesting-tesla-w1xtr?file=/index.js

  ```
  //index.html
  <!DOCTYPE html>
  <html lang="en">
  
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="./index.js"></script>
    <style>
      #outer {
        padding: 20px;
        background: pink;
      }
  
      #inner {
        width: 200px;
        height: 200px;
        line-height: 200px;
        text-align: center;
        background: skyblue;
      }
    </style>
    <title>宏任务、微任务</title>
  </head>
  
  <body>
    <div id="outer">
      <div id="inner">爱我你就点点我</div>
    </div>
  </body>
  
  </html>
  ```

  ```
  //index.js
  window.onload = function () {
    const $inner = document.getElementById('inner')
    const $outer = document.getElementById('outer')
  
    function handler() {
      console.log('click') // 直接输出
  
      Promise.resolve().then(_ => console.log('promise')) // 注册微任务
  
      setTimeout(_ => console.log('timeout')) // 注册宏任务
  
     //页面重绘之前做的操作
      requestAnimationFrame(_ => console.log('animationFrame')) // 注册宏任务
  
      $outer.setAttribute('data-random', Math.random()) // DOM属性修改，触发微任务
    }
  
    new MutationObserver(_ => {  //监听DOM树进行的更改
      console.log('observer')
    }).observe($outer, {
      attributes: true
    })
  
    $inner.addEventListener('click', handler)
    $outer.addEventListener('click', handler)
  }
  ```

  ![](1.gif)

  浏览器输出：

  ![](Snipaste_2020-06-17_19-57-17.png)

  分析一下结果：

   `click` ->`promise`->`observer` 两次同样的输出是因为事件冒泡

### 在node中的表现

 [Node](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/) 中的 Event Loop 和浏览器中的是完全不相同的东西。Node.js 采用 V8 作为 js 的解析引擎，而 I/O 处理方面使用了自己设计的 libuv，libuv 是一个基于事件驱动的跨平台抽象层，封装了不同操作系统一些底层特性，对外提供统一的 API，事件循环机制也是它里面的实现。

 **libuv引擎中事件循环的六个阶段**

![img](Snipaste_2020-06-19_20-45-56.png)

#### setImmediate 和 setTimeout

 `setTimeout`的回调函数在check阶段执行，`setTimeout`的回调函数执行的条件是`poll`阶段是空闲，且达到设定时间在`timer`阶段执行。这两个函数的执行，先后顺序不一样。

javascript

```javascript
setTimeout(function timeout () {
  console.log('timeout');
},0);
setImmediate(function immediate () {
  console.log('immediate');
});
```

 但如果在`i/o`操作中执行，一定是`setImmediate`先执行，是因为`poll`阶段执行的是`i/o`操作，接下来就是`check`阶段。

javascript

```javascript
const fs = require('fs')
fs.readFile(__filename, () => {
    setTimeout(() => {
        console.log('timeout');
    }, 0)
    setImmediate(() => {
        console.log('immediate')
    })
})
```

![img](Snipaste_2020-06-19_20-54-29.png)

#### process.nextTick

 这个函数其实是独立于 Event Loop 之外的，它有一个自己的队列，当每个阶段完成后，如果存在 nextTick 队列，就会清空队列中的所有回调函数，并且优先于其他 microtask 执行。

- 栗子一

  ```
  let bar;
  
  function someAsyncApiCall(callback) { callback(); }
  
  someAsyncApiCall(() => {
    console.log('bar', bar); // undefined
  });
  
  bar = 1;
  ```

  js

  ```js
  let bar;
  
  function someAsyncApiCall(callback) {
    process.nextTick(callback);
  }
  
  someAsyncApiCall(() => {
    console.log('bar', bar); // 1
  });
  
  bar = 1;
  ```

  第一个代码会输出，`undifined`，第二个代码会输出`1`，是因为 `process.nextTic`会等待当前操作完之后在执行，也就是等到第二个代码中的赋值操作完成之后在执行回调函数。

- 栗子二

  ```
  setTimeout(() => {
   console.log('timer1')
   Promise.resolve().then(function() {
     console.log('promise1')
   })
  }, 0)
  process.nextTick(() => {
   console.log('nextTick')
   process.nextTick(() => {
     console.log('nextTick')
     process.nextTick(() => {
       console.log('nextTick')
       process.nextTick(() => {
         console.log('nextTick')
       })
     })
   })
  })
  // nextTick=>nextTick=>nextTick=>nextTick=>timer1=>promise1
  ```

  跟第一个栗子有点点区别，在第一个栗子中，都是同步代码块，而第二个栗子中`setTimeout`中的回调是异步的，它会由于`settimeout`的回调执行。

#### 关于node 与 浏览器的Event Loop的区别

 最新的node版本，在运行结果上跟浏览器运行结果是一样的，网上说这两者之间运行存在差异，是因为node版本的问题，更新一下就好了，实测。




