---
title: 微前端-Vue实践
top: false
cover: false
toc: true
mathjax: true
date: 2021-02-20 17:02:37
password:
summary: 微前端-Vue3.0实践
tags: 实践
categories: 微前端
---

#  微前端-Vue3.0实践

## 什么是微前端

微前端就是将不同的功能按照不同的维度拆分成多个子应用。通过主应用来加载这些子应用，核心在于拆，拆完在合。

## 为什么使用微前端

1. 忽略技术栈区别
2. 子应用可以单独打包部署
3. 可以兼容老代码

## 怎么落地

### Single-Spa实现

[Single-SPA](https://github.com/single-spa/single-spa)是一个用于前端微服务化的JavaScript前端解决方案（没有处理样式隔离、js隔离）实现了路由劫持和应用加载。

先上一张图，这是基于Single-Spa实现的效果：

![](https://i.loli.net/2021/02/22/kyoO9SzKu4XmEvY.gif)

1. 创建两个vue工程（我是通过vue cli创建的）

``` 
vue create parent-app // 创建父应用
vue create child-app // 创建子应用
两个工程自定义只选择了 bable 和 vueRouter
```

2. 改造子应用

   ```
   子应用需要暴露出 bootstrap mount unmount 方法
   ```

   + 子应用安装`single-spa-vue`：`npm insatll sigle-spa-vue`，在`main.js`里面引入`single-spa-vue`，singleSpa会重新包装Vue，返回的对象包含子应用需要导出的三个方法。

   ![](https://i.loli.net/2021/02/22/IOZ4acA5CqdbhTB.png)

   + 新建`vue.config.js`文件，output设置是将打包输出成umd格式，被父应用引用，deveServer设置了启动的端口号

   ![](https://i.loli.net/2021/02/22/ctSJGdmxWYqOnLA.png)

3. 改造父应用

   + 父应用安装`single-spa`：`npm install single-spa`，父应用需要注册并挂载子应用。

     ![](https://i.loli.net/2021/02/22/BWaeHyJKj5pxQ7w.png)

     引入`single-spa`，`registerApplication` 是注册子应用，第一个参数是子应用的名字，第二个参数是一个方法，主要是加载子应用打包出来的结果，注意加载的顺序先加载公共模块，在加载app.js，不然就会出现下面的报错。返回值包含了子应用导出的三个方法。

     ![](https://i.loli.net/2021/02/22/38FmfO5uAadIMgr.png)

     第三个参数是触发加载子应用的条件：当路由匹配到开头是`subapp`就加载子应用。当然可以传入自定义参数，具体参考[这个网址](https://single-spa.js.org/docs/api)。

     start方法是加载注册的子应用。

     + App.vue 需要提供子应用的挂载点，`id`跟子应用`appOptions`的`el`属性相对应。

   ![](https://i.loli.net/2021/02/22/34GgFc8CKMLvtZ7.png)

   到这里基本上就可以跑起来了，但还是会遇到一些问题：子应用的路由点击不了 和 子路由中的图片也显示不了。

   ![](https://i.loli.net/2021/02/22/FvcWQtTYG9gOB4C.gif)

4. 细节修正

   上面的问题是应为子应用路由跳转没有基于自身跳转以及请求资源的路径不对（图片显示不出来）

   在子应用的路由配置中加入base选项，解决的是路由跳转的问题

   ![](https://i.loli.net/2021/02/22/GpNt4dWmxsuIz7X.png)

   `main.js`中加入如下判断，如果被主应用引用的话，资源请求路径以配置的为主

   ![](https://i.loli.net/2021/02/22/8kgpKJVTicvDb79.png)

   这样的话基于`Single-Spa`的微前端应用已经实现了

### Single-Spa 缺陷

基于single-spa实现的微前端还存在几个问题，样式隔离以及js隔离。在上面的动图演示中，当加载子应用的时候，父应用的样式就被影响了。共用全局对象(window)就造成了全局对象的污染，接下来就针对这两点解决问题。

#### 样式隔离

+ BEM（Block Element Modifier）约定项目前缀（约定仅仅是约定而已）
+ CSS-Modules 打包时生成不冲突的选择器名
+ Shadow DOM 真正意义的隔离
+ Css-in-js

上面这这几种方法都可以解决样式不隔离的问题，以`Shadow DOM`为例：

上图：

![](https://i.loli.net/2021/02/22/uaKw6keZOo8Frxn.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>样式隔离</title>
</head>
<body>
    <div>
        <p>我是P标签</p>
        <div id="shadow"></div>
    </div>
    
    <script>
        let shadowDOM = document.getElementById('shadow').attachShadow({mode:'closed'})
        let pEle = document.createElement('p')
        let shadowStyles = document.createElement('style')
        let commonStyles = document.createElement('style')
        shadowStyles.textContent = 'p{color:red}'
        commonStyles.textContent = 'p{color:green}'
        pEle.innerText = '我是shadow P'
        shadowDOM.appendChild(pEle)
        shadowDOM.appendChild(shadowStyles)
        document.body.appendChild(commonStyles)
    </script>
</body>
</html>
```

Shadow API是dom对象自带的，shadow里面的元素外界不能访问

#### Js隔离

沙箱： 快照沙箱（单应用沙箱，浅拷贝） 代理沙箱（多应用沙箱，es6 proxy）

沙箱目的是为了实现逻辑上的隔离，一个应用的逻辑变化不会影响其他应用

##### 单应用

只有一个应用实例可以使用浅拷贝将快照记录下来

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>js沙箱隔离</title>
</head>
<body>
    <script>
        class SnapshotSandbox{
            constructor(){
                this.proxy = window
                this.modifyProps = {} // 记录变化量
                this.snapshot = {}
                this.active()
            }
            active(){
                for(const prop in window){
                    this.snapshot[prop] = window[prop]
                }
                Object.keys(this.modifyProps).forEach(p=>{
                    window[p] = this.modifyProps[p]
                })
            }
            inactive(){
                for(const prop in window){
                    if(this.snapshot[prop] !== window[prop]){
                        this.modifyProps[prop] = window[prop]
                        window[prop] = this.snapshot[prop] // 确保不会影响
                    }
                }
            }
        }
        let sandbox = new SnapshotSandbox()
        function start(window){
            window.a = 1,window.b = 2
            console.log(window.a,window.b)
            sandbox.inactive()
            console.log(window.a,window.b)
            sandbox.active()
            console.log(window.a,window.b)
        }
       start(sandbox.proxy)
    </script>
</body>
</html>
```



![](https://i.loli.net/2021/02/22/GfgM3WiuV29CF7w.png)

##### 多应用

存在多个应用实例的话，可以借助`es6 proxy`实现js隔离

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>多应用JS隔离</title>
</head>
<body>
    <script>
        class ProxySandbox{
            constructor(){
                const rawWindow = window
                const fakeWindow = {}
                const proxy = new Proxy(fakeWindow,{
                    set(target, p ,value){
                        target[p] = value
                        return true
                    },
                    get(target, p){
                        return target[p] || rawWindow[p]
                    }
                })
                this.proxy = proxy
            }
        }
        let sandbox1 = new ProxySandbox()
        let sandbox2 = new ProxySandbox()
        window.a = 1
        function start(window,value){
            window.a = value
            console.log(window.a)
        }
     start(sandbox1.proxy,'hello sandbox1')
    start(sandbox2.proxy,'hello sandbox2')
    </script>
</body>
</html>
```

![](https://i.loli.net/2021/02/22/U4Ype1XmfLbPH9r.png)

Bingo......

### QianKun实现

[qiankun](https://github.com/umijs/qiankun)基于Single-SPA，提供了开箱即用的API（single-spa	+	sandbox	+	import-html-entry）做到的，技术栈无关，接入简单（像iframe）。

![](https://i.loli.net/2021/02/22/b5rGPsCj8cUyhY6.gif)

上图效果是基于qinakun2.0^ + vue3.0^ + antd-vue + mockJs + TS等技术实现的，一个主应用+ 两个子应用（我的代码和生活快照），当然两个子应用也可以独立运行。主要从以下几个方面分享：

+ 主、子应用的构建
+ 应用间的通信
+ mock数据
+ 根据环境（local、develop、 product）的不同动态mock数据
+ 权限控制

#### 构建主、子应用

![](https://i.loli.net/2021/02/22/DqzetjEHX1QBVdZ.png)

​																											结构

##### 构建主应用

main.ts

```javascript
import { createApp, h } from 'vue'
import { registerMicroApps, runAfterFirstMounted, setDefaultMountApp, start } from 'qiankun'
import { message } from 'ant-design-vue'
import actions from './shared/actions'
import loadBaseUiComponent from './plugins/antd'
import loadOneUiComponent from './library/ui/install'
import OneUi from './library/ui'
import App from './App.vue'
import router from './router'
import store from './store'

let app: any = null
const msg = { 
    data: [],
    uiComponent: OneUi,
    actions,
}

function render({ appContent, loading }: { appContent?: any; loading?: any } = {}): void {
    if (!app) {
        app = createApp({
            data() {
                return {
                    content: appContent,
                    loading,
                }
            },
            render() {
                return h(App, {
                    props: {
                        content: this.content,
                        loading: this.loading,
                    },
                })
            },
        })
    } else {
        app.content = appContent
        app.loading = loading
    }
    app.use(router).use(store)
    loadBaseUiComponent(app)
    loadOneUiComponent(app)
    app.mount('#contain')
}

render()
function getActiveRule(routerPrefix: string) {
    return (location: { pathname: string }) => location.pathname.startsWith(routerPrefix)
}
registerMicroApps(  //注册子应用
    [
        {
            name: 'sub-app-code',
            entry: '//localhost:8895',
            container: '#sub-app-view', 
            activeRule: getActiveRule('/code'), // 激活子应用的条件
            props: msg, 
        },
        {
            name: 'sub-app-life',
            entry: '//localhost:8099',
            container: '#sub-app-view',
            activeRule: getActiveRule('/life'),
            props: msg,
        },
    ],
    {
        beforeLoad: [
            (currentApp) => {
                console.log('before load', currentApp)
                return Promise.resolve()
            },
        ], // 挂载前回调
        beforeMount: [
            (currentApp) => {
                console.log('before mount', currentApp)
                return Promise.resolve()
            },
        ], // 挂载后回调
        afterUnmount: [
            (currentApp) => {
                console.log('after unload', currentApp)
                return Promise.resolve()
            },
        ],
    },
)

setDefaultMountApp('/code')
runAfterFirstMounted(() => {})
start()
```

App.vue

```html
<div class="sub-app-view" id="sub-app-view" style="height: 100%"></div> //提供挂载点
```

相对于`single-spa`实现中构建主应用，`qiankun`可以动态的获取子应用打包的结果

##### 构建子应用

main.ts

导出三大金刚`bootstrap mount unmount`

```javascript
/* eslint-disable no-underscore-dangle */
import { createApp } from 'vue'
import loadBaseUiComponent from './plugins/antd'
import loadOneUiComponent from './library/ui/install'
import './utils/micro/public-path'
import App from './App.vue'
import actions from './shared/actions'
import router from './router'
import store from './store'

let instance: any = null
export async function bootstrap(props = {}) {
    console.log(props)
}
export async function mount(props: any) {
    console.log(props)
    instance = createApp(App)
        .use(router)
        .use(store)
    loadBaseUiComponent(instance)
    loadOneUiComponent(instance)
    instance.mount('#sub-app-code')
}
export async function unmount() {
    instance.$destroy?.()
    instance = null
}

// eslint-disable-next-line no-unused-expressions
window.__POWERED_BY_QIANKUN__ || mount({})
```

vue.config.js

与`single-spa`构建不同的是这里需要对跨域进行设置

```javascript
 devServer: {
    // host: '0.0.0.0',
    hot: true,
    disableHostCheck: true,
    port,
    overlay: {
      warnings: false,
      errors: true
    },
    headers: {
      'Access-Control-Allow-Origin': '*'   //跨域设置
    }
  },
   configureWebpack: {
    devServer: {
      open: true,
    },
    output: {
      // 把子应用打包成 umd 库格式
      library: 'sub-app-code',
      libraryTarget: 'umd'
    },
  },
    
```

router的配置也要根据是否在qiankun环境里面动态设置

```javascript
const router = createRouter({
    history: createWebHistory(__qiankun__ ? '/code' : '/'), // todo /code 需要主应用下发
    routes,
})
```

`single-spa`构建子应用对``__webpack_public_path__``进行设置，在`qiankun`中也要进行设置，抽离到了`utils/micro/public-path.js`下

```javascript
/* eslint-disable camelcase */
/* eslint-disable no-underscore-dangle */
// eslint-disable-next-line no-underscore-dangle
if (window.__POWERED_BY_QIANKUN__) {  // window.__POWERED_BY_QIANKUN__ 是否是qiankun环境的标志
  // eslint-disable-next-line no-undef
  __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__
}
```

这样一个基于`qiankun`的微前端应用就构建完成了

#### 应用间通讯

我使用官方提供的`actions通信`，`qiankun` 内部提供了 `initGlobalState` 方法用于注册 `MicroAppStateActions` 实例用于通信，该实例有三个方法，分别是：

- `setGlobalState`：设置 `globalState` - 设置新的值时，内部将执行 `浅检查`，如果检查到 `globalState` 发生改变则触发通知，通知到所有的 `观察者` 函数。
- `onGlobalStateChange`：注册 `观察者` 函数 - 响应 `globalState` 变化，在 `globalState` 发生改变时触发该 `观察者` 函数。
- `offGlobalStateChange`：取消 `观察者` 函数 - 该实例不再响应 `globalState` 变化。

效果就是`globalState`变化后，会有提示框出现

![](https://i.loli.net/2021/02/22/KeXjSfb9LQUREh8.png)

1. 创建actions实例（主应用里面操作）

   `shared/actions.ts`

   ```javascript
   import { initGlobalState, MicroAppStateActions } from 'qiankun'
   
   const initialState = {}
   const actions: MicroAppStateActions = initGlobalState(initialState)
   
   export default actions
   ```

2. 观察globalState变化

   `main.ts`

   ```javascript
   function render({ appContent, loading }: { appContent?: any; loading?: any } = {}): void {
      ...
       actions.onGlobalStateChange((state, preState) => {
           console.log('new', state, 'old', preState)
           message.success(`新消息提醒：${state.message}`) //当globalState变化会有提示框出现
       })
     ...
   }
   ```

3. 下发actions

   目前只有主应用使用了`actions`，通信的话肯定不能一个人solo，我们需要将`actions`下发发到子应用，也很简单在需要在传递给子应用的props多传一个参数即可

   `main.ts`

   ```javascript
   const msg = {
       data: [],
       uiComponent: OneUi,
       actions, //actions 实例
   }
   registerMicroApps(
       [
           {
               name: 'sub-app-code',
               entry: '//localhost:8895',
               container: '#sub-app-view',
               activeRule: getActiveRule('/code'),
               props: msg, //下传给子应用
           },
           {
               name: 'sub-app-life',
               entry: '//localhost:8099',
               container: '#sub-app-view',
               activeRule: getActiveRule('/life'),
               props: msg,
           },
       ],
       ...
   )
   ```

   需要注意的是，在子应用里面需要构建`actions类`，接收主应用传来的`actions`

   ```javascript
   class Actions {
       actions = {
           onGlobalStateChange: (args: any) => args,
           setGlobalState: (args: any) => args,
       }
   
       /**
        * 设置 actions
        */
       // eslint-disable-next-line no-unused-vars
       setActions(actions: { onGlobalStateChange: (args: any) => any; setGlobalState: (args: any) => any }) {
           this.actions = actions
       }
   
      
       onGlobalStateChange(args: any) {
           return this.actions.onGlobalStateChange({ ...args })
       }
   
     
       setGlobalState(args: any) {
           return this.actions.setGlobalState({ ...args })
       }
   }
   
   const actions = new Actions()
   export default actions
   
   ```

   子应用的 `main.ts`  mount方法调用

   ```javascript
   export async function mount(props: any) {
       if (props.actions) {
           actions.setActions(props.actions)
           actions.setGlobalState({ message: 'BBB' })
       }
      ...
   }
   ```

   Bingo...

   #### mock数据和根据不同环境对请求拦截

   使用的是`mockjs`。根目录下创建这三个文件就可以设置不同环境的标志

   ![](https://i.loli.net/2021/02/22/CHryLN2ou65x143.png)

   例如： .env.dev-local

   ```
   VUE_APP_CURRENTMODE = 'dev-local'
   VUE_APP_ENV = '本地开发环境'
   VUE_APP_MOCK = true
   ```

   当然还需要设置一下 npm script，例如我想运行本地环境命令就是 ： `npm run dev-local`，它会读取` .env.dev-local`里面的值加入到全局环境变量里面去。

   ```json
    "dev-local": "vue-cli-service serve --mode dev-local",
   "dev-remote": "vue-cli-service serve --mode dev-remote",
   ```

   然后就可以根据你想要的环境动态开启mock了，我是这样做的，在`main.ts`里面调用`useMock()`

   ```javascript
   function useMock() {
       console.log('process.env', process.env)
       if (process.env.VUE_APP_MOCK) {
           // eslint-disable-next-line global-require
           require('../mock')
       }
   }
   ```

   #### 权限控制

   可以统一在主应用请求路由权限，然后下发到子应用去，就可以做到权限控制了。

   ~~我没写~~

```
总结：子应用可以单独构建，运行时动态加载。主、子应用完全解耦，技术栈无关，靠的是协议接入（子应用必须导出bootstrap、mount、unmount方法）
```

## 结尾

### 为什么不使用iframe

用户刷新页面会丢失当前状态

源码：  [qiankun版本](https://github.com/muyeyong/micro-qiankun)      [single-spa](https://github.com/muyeyong/micro-singleSpa)

