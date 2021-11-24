---
title: JavaScript-怎么跳出Array.forEach()
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-19 20:40:15
password:
summary:
tags: 小技巧
categories: JavaScript
---

# JavaScript-怎么跳出Array.forEach()

> forEach()会遍历完整个数组，不会中途退出

## 抛出异常

```
function foreach(a, f, t) {
  try {
    a.forEach(f, t)
  }
  catch (e) {
    if (e === foreach.break) return;
    else throw e;
  }
}

foreach.break = new Error('StopInteration');

arr = [1, 2, 3, 4, 5]

foreach(arr, x => { if (x === 3) throw foreach.break; else console.log(x) })

//输入 1,2
```

##  使用forEach的第二个参数

```
[1, 2, 3, 4].forEach(x => {
  if (this.break == true) {
    return;
  }
  if (x === 2) this.break = true;
  console.log(x)
}, {})
```

## 改变数组本身

```
arr = [1, 2, 3, 4, 5]

arr.forEach((v, index) => {
  if (v > 2) {
    arr.splice(index, arr.length - index)
  }
  console.log(v)
})

//输出 1,2,3
```

## 放大招了

**用some() ,for循环不香吗？？**