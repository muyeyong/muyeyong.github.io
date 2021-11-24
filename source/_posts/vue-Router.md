---
title: vue Router
top: false
cover: false
toc: true
mathjax: true
date: 2020-09-30 10:29:30
password:
summary: 导航狗
tags: Router
categories: vue-Router
---

# vue Router

[Vue Router](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html)

## 基础知识

注入路由后，可以通过`this.$router`访问路由器，`this.$route`访问当前路由

## 动态路由

### 匹配特定路径

```
routers:[psth:'/user/:id',component: User]
```

`/user/A`、`/user/B`都将匹配到相同的路由

动态路径参数使用`:`标记，可以通过`this.$route.params`，例如访问上面的`id`：`this.$route.params.id`

从`/user/A`转到`/user/B`的时候，组件将被复用，生命周期将不会被调用，如果想根据路由参数进行响应的话：

1. 监测`$route`对象

   ```
   watch:{
   	$route(to,from){
   		//do something
   	}
   }
   ```

   

2. 使用路由守卫

```
beforeRouteUpdate(to,from,next){
	//do something
}
```

### 匹配任意路径（捕获404）

使用通配符`*`，当使用通配符的时候，`$route.params`会自动添加一个名为`pathMatch`的参数，包含通过通配符被匹配的部分

### 匹配优先级

同一个路径可以匹配多个路由，匹配的优先级是：谁先匹配，谁的优先级更高

## 嵌套路由

`<router-view></router-view>`是最顶层的出口，渲染最高级路由匹配的组件，被渲染的组件也可以包含`<router-view></router-view>`，需要配置`children`配置：

```
routers:[psth:'/user/:id',component: User,
					children:[{
						path:'', component: 
					},
					.....]
]
```

## 命名路由

可以通过一个名称来标识一个路由

```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
}
```

```
//声明式
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
//编程式
router.push({ name: 'user', params: { userId: 123 }})
```



## 编程式路由

```
router指代this.$router
```

### router.push(`location, onComplete?, onAbort?`)

| 声明式                  | 编程式           |
| ----------------------- | ---------------- |
| <router-link :to="..."> | router.push(...) |

参数可以是字符串路径或描述地址的对象

```
//字符串
router.push('home')
//对象
router.push({path:'home'})
//命名的路由
router.push({name:'user',params:{userId:'123'}})
//带查询参数
router.push({path:'register',query:{plan:'private'}})
```

如果提供了`path`，`params`会被忽略掉

```
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

同样的规则也适用于`router-link`的`to`属性

### router.repace(`location, onComplete?, onAbort?`)

| 声明式                          | 编程式              |
| ------------------------------- | ------------------- |
| <router-link :to="..." replace> | Router.replace(...) |

不会向history里面添加记录

### onComplete 和 onAbort

导航成功完成 (在所有的异步钩子被解析之后) 或终止 (导航到相同的路由、或在当前导航完成之前导航到另一个不同的路由) 的时候进行相应的调用

### router.go(n)

在history记录中前进或后退多少步，类似于window.history.go(n)

## 对比history

|     Vue router      |             history              |
| :-----------------: | :------------------------------: |
|  router.push(...)   |  window.history.pushState(...)   |
| router.replace(...) | window.history.repalceState(...) |
|    router.go(n)     |       window.history.go(n)       |

## 命名视图

同级展示多个视图，可以在界面中拥有多个单独命名的视图，而不是一个出口，如果`router-view`没有设置名字，那么默认为`dafault`

```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>

const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

## 重定向和别名

重定向通过`routes`来配置

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

`redirect`也可以是一个命名的路由，也可以是一个方法

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})

const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

别名对应的配置如下：

```
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

## 路由组件传参

在组件中直接使用`$route`会使其与对应路由形成高度耦合，使用`props`将组件和路由解耦

```
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

https://jsfiddle.net/posva/6du90epg/

## hash和history模式（针对单页应用SPA）

```
前端路由的核心，就在于 —— 改变视图的同时不会向后端发出请求。
```

hash——即地址栏 URL 中的 # 符号（此 hash 不是密码学里的散列运算）。 比如这个 URL：http://www.abc.com/#/hello，hash 的值为 #/hello。它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

已经有 hash 模式了，而且 hash 能兼容到IE8， history 只能兼容到 IE10，为什么还要搞个 history 呢？
首先，hash 本来是拿来做页面定位的，如果拿来做路由的话，原来的锚点功能就不能用了。其次，hash 的传参是基于 url 的，如果要传递复杂的数据，会有体积的限制，而 history 模式不仅可以在url里放参数，还可以将数据存放在一个特定的对象中。

## 进阶

### 导航守卫

> 导航表示路由正在发生变化，查询参数的变化并不会触发进入/离开的导航守卫

* 全局前置守卫

```
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

当一个导航触发的时，全局前置守卫按照创建顺序调用

next:Function:一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。

​	**`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。

​	**`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。

​	**`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。

​	**`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

* 全局后置钩子

```
router.afterEach((to, from) => {
  // ...
})
```

* 路由独享守卫

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

* 组件内的守卫
  * beforeRouterEnter
  * beforeRouterUpdate
  * beforeRouterLeaver

```
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

```
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

