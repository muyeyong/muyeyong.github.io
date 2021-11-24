---
title: React-生命周期加载顺序引发的思考
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-28 11:20:28
password:
summary:
tags: 生命周期
categories: React
---

# React生命周期加载顺序引发的思考

​	在写毕业设计的时候用到了`componentWillMount(已经不推荐使用了)`  和`componentDidMount`两个生命周期，`componentWillMount`  里面写了请求， `componentDidMount`里面也写了请求（~~原谅我的骚操作~~），而`componentDidMount`里面的请求依赖`componentWillMount`里面的请求（~~是不是很骚~~），但是`componentDidMount`并没有接收到`componentWillMount`请求来的数据，我就有点纳闷，按照加载顺序不应该是这样的，模拟一下当时的情况：

![](Snipaste_2020-04-28_11-47-24.png)

​	它会按生命周期的顺序加载，但不会同步等待完成在进行另一个生命周期，还有请求数据的话，一般放到`componentDidMount`里面去执行，问题来了，为什么要放在`componentDitMount`里面去请求数据。

​	React16以后采用了 `Fiber` 架构，一次更新过程被分为很多的片段，不在是一开始更新就不能停止的状态，会存在一个更新片段还没更新完，就被另一个优先级更高的片段取代，导致优先级相对较低的片段的更新任务完全废止，直到下一次被调用。

```
采用fiber的原因以及优先级的解释：
	React的渲染分为两个阶段：
		调度阶段： 更新数据，生成虚拟DOM，根据diff算法查出需要更新的真实DOM，放入更新队列
		渲染阶段： 将更新队列的数据一次性更新到真实DOM上
	调度阶段是不可控的，整个阶段的主线程都被占用，这个阶段用户的操作得不到反馈，只有当React中的同步更新完成之后主线程才会被释放。
	fiber会让我们获得以下权限：
		1：暂停运行的任务
		2：恢复并继续执行任务
		3：确定优先级
	fiber创建了自己的堆栈帧，而没有使用JavaScript的堆栈帧，就可以使上述的权限成为可能，将调度任务拆分成一小段并设定执行时间。
	优先级的确定：
		文本框输入 > 本次调度结束需完成的任务 > 动画过渡 > 交互反馈 > 数据更新 > 不会显示但以防将来会显示的任务
```

调度阶段的生命周期：

- construct
- getDerivedStateFromProps
- shouldComponentUpdate
- render
- ~~componentWillMount~~
- ~~componentWillReceiveProps~~
- ~~componentWillUpdate~~

**加删除线的生命周期是准备废弃的**

渲染阶段的生命周期：

- componentDidMount

- componentDidUpdate

- componentWillUnmount

  由于调度阶段的更新任务可能被打断重新开始，所以调度阶段的生命周期可能会执行多次。

纵向分析的话，生命周期分为三个阶段：

1. 挂载阶段

- constructor: 构造函数，最先被执行,我们通常在构造函数里初始化state对象或者给自定义方法绑定this
- getDerivedStateFromProps: `static getDerivedStateFromProps(nextProps, prevState)`,这是个静态方法,当我们接收到新的属性想去修改我们state，可以使用getDerivedStateFromProps
- render: render函数是纯函数，只返回需要渲染的东西，不应该包含其它的业务逻辑,可以返回原生的DOM、React组件、Fragment、Portals、字符串和数字、Boolean和null等内容
- componentDidMount: 组件装载之后调用，此时我们可以获取到DOM节点并操作，比如对canvas，svg的操作，服务器请求，订阅都可以写在这个里面，但是记得在componentWillUnmount中取消订阅

1. 更新阶段
   - getDerivedStateFromProps: 此方法在更新个挂载阶段都可能会调用
   - shouldComponentUpdate: `shouldComponentUpdate(nextProps, nextState)`,有两个参数nextProps和nextState，表示新的属性和变化之后的state，返回一个布尔值，true表示会触发重新渲染，false表示不会触发重新渲染，默认返回true,我们通常利用此生命周期来优化React程序性能
   - render: 更新阶段也会触发此生命周期
   - getSnapshotBeforeUpdate: `getSnapshotBeforeUpdate(prevProps, prevState)`,这个方法在render之后，componentDidUpdate之前调用，有两个参数prevProps和prevState，表示之前的属性和之前的state，这个函数有一个返回值，会作为第三个参数传给componentDidUpdate，如果你不想要返回值，可以返回null，此生命周期必须与componentDidUpdate搭配使用
   - componentDidUpdate: `componentDidUpdate(prevProps, prevState, snapshot)`,该方法在getSnapshotBeforeUpdate方法之后被调用，有三个参数prevProps，prevState，snapshot，表示之前的props，之前的state，和snapshot。第三个参数是getSnapshotBeforeUpdate返回的,如果触发某些回调函数时需要用到 DOM 元素的状态，则将对比或计算的过程迁移至 getSnapshotBeforeUpdate，然后在 componentDidUpdate 中统一触发回调或更新状态
2. 卸载阶段
   - componentWillUnmount: 当我们的组件被卸载或者销毁了就会调用，我们可以在这个函数里去清除一些定时器，取消网络请求，清理无效的DOM元素等垃圾清理工作

补一张最新的生命周期图：

查看React生命周期的网站：https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

![](Snipaste_2020-04-28_15-11-30.png)