---
title: React-大纲
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-09 11:42:51
password:
summary:
tags: 大纲
categories: React
---

## setState是不是异步

> 有时表现异步有时表现同步

合成事件和钩子函数中是异步，原生事件和`setTimeout`中是同步

 合成事件？ 原生事件？

如果DOM上绑定了过多的事件处理函数，整个页面响应以及内存占用可能都会受到影响。React为了避免这类DOM事件滥用，同时屏蔽底层不同浏览器之间的事件系统差异，实现了一个中间层——SyntheticEvent。

 SyntheticEvent ： 跨浏览器原生事件包装器

 在事件回调被调用后，`SyntheticEvent` 对象将被重用并且所有属性都将被取消

React并不是将click事件绑在该div的真实DOM上，而是在document处监听所有支持的事件，当事件发生并冒泡至document处时，React将事件内容封装并交由真正的处理函数运行。

![](1.png)

target 和 currentTarget 的区别 ？

浏览器事件过程： 捕获 - 目标 - 冒泡 ？

## React中获取DOM节点

- ReactDom.findDOMNode(component)（React组件的引用）

```
//引入
import ReactDOM from 'react-dom'
//使用
componentDidMount(){
    const dom = ReactDOM.findDOMNode(this);
    // this为当前组件的实例
}
render() {}
```

- refs用于React组件内子组件的引用

```
// ref方法
<div ref={ref=> this.div = ref} />    //通过this.div获取

//refs方法
<div ref='div' />    //通过this.refs.div获取

//React.creatRef()
this.div = React.createRef() //constructor
<div ref={this.div} /> //render  通过this.div获取
```

## 组件/逻辑的复用

- 高阶组件（不是特别清晰了）

 属性代理

 反向继承

组件将props转换成UI，高阶组件将组件转换成另一个组件

- 渲染属性（Render Props）/ 容器组件
- react-hooks

## redux工作流程

- Store
- State
- Action
- Reducer
- dispatch

## react-redux

- Provider
- connect

## redux 和 mobx的区别

**两者对比:**

- redux将数据保存在单一的store中，mobx将数据保存在分散的多个store中
- redux使用plain object保存数据，需要手动处理变化后的操作；mobx适用observable保存数据，数据变化后自动处理响应的操作
- redux使用不可变状态，这意味着状态是只读的，不能直接去修改它，而是应该返回一个新的状态，同时使用纯函数；mobx中的状态是可变的，可以直接对其进行修改
- mobx相对来说比较简单，在其中有很多的抽象，mobx更多的使用面向对象的编程思维；redux会比较复杂，因为其中的函数式编程思想掌握起来不是那么容易，同时需要借助一系列的中间件来处理异步和副作用
- mobx中有更多的抽象和封装，调试会比较困难，同时结果也难以预测；而redux提供能够进行时间回溯的开发工具，同时其纯函数以及更少的抽象，让调试变得更加的容易

**场景辨析:**

基于以上区别,我们可以简单得分析一下两者的不同使用场景.

mobx更适合数据不复杂的应用: mobx难以调试,很多状态无法回溯,面对复杂度高的应用时,往往力不从心.

redux适合有回溯需求的应用: 比如一个画板应用、一个表格应用，很多时候需要撤销、重做等操作，由于redux不可变的特性，天然支持这些操作.

mobx适合短平快的项目: mobx上手简单,样板代码少,可以很大程度上提高开发效率.

当然mobx和redux也并不一定是非此即彼的关系,你也可以在项目中用redux作为全局状态管理,用mobx作为组件局部状态管理器来用.