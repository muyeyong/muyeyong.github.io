---
title: 从零开始-搭建React脚手架
top: false
cover: false
toc: true
mathjax: true
date: 2020-05-29 11:36:16
password:
summary:
tags: React脚手架搭建
categories: 从零开始
---

# 从零开始-React脚手架搭建

目标：

- 识别`TSX`文件

- `tree shaking` 摇树优化 删除掉无用代码T

- 识别 `async / await `和 箭头函数

- `PWA`功能，热刷新，安装后立即接管浏览器 离线后仍让可以访问网站 还可以在手机上添加网站到桌面使用

- `preload` 预加载资源 `prefetch`按需请求资源

- `CSS`模块化，不怕命名冲突

- 小图片的`base64`处理

- 文件后缀省掉`jsx js json`等

- 实现React懒加载，按需加载 ， 代码分割 并且支持服务端渲染

- 支持`less sass stylus`等预处理

- `code spliting` 优化首屏加载时间 不让一个文件体积过大

- 加入`dns-prefetch`和`preload`预请求必要的资源，加快首屏渲染。

- 加入`prerender`，极大加快首屏渲染速度。

- 提取公共代码，打包成一个`chunk`

- 每个`chunk`有对应的`chunkhash`,每个文件有对应的`contenthash`,方便浏览器区别缓存

- 图片压缩

- `CSS`压缩

- 增加`CSS`前缀 兼容各种浏览器

- 对于各种不同文件打包输出指定文件夹下

- 缓存`babel`的编译结果，加快编译速度

- 每个入口文件，对应一个`chunk`，打包出来后对应一个文件 也是`code spliting`

- 删除`HTML`文件的注释等无用内容

- 每次编译删除旧的打包代码

- 将`CSS`文件单独抽取出来

- 让babel不仅缓存编译结果，还在第一次编译后开启多线程编译，极大加快构建速度

- webpack监听文件自动增量编译

  



源代码： https://github.com/muyeyong/webck-gulp-temp





疑问：

1. package.json
   + dependencies 和 devDependencies 区别

2. npm 命令 和 npm 全局安装 和 本地安装的区别

神经病报错：

​	npm i -g webpack webpack-cli

![](Snipaste_2020-05-29_15-21-22.png)

提示我 webpack-cli，然而当我运行 `webpack --config ./build/webpack.base.js --mode=development` 的时候有报错 no found webpack-cli，不懂就上google，一顿操作猛如虎，代码运行就是error，嘤嘤嘤，最后我就把`C:\Users\admin\AppData\Roaming\npm` 这个路径下的这两个文件给删掉![](Snipaste_2020-05-29_15-25-14.png)

出现了新报错，真开心。。。。

![](Snipaste_2020-05-29_15-26-29.png)

React 里面使用 TS

​	报错识别不了，react （参考文章） https://juejin.im/post/5bab4d59f265da0aec22629b

​	![](Snipaste_2020-05-29_15-43-40.png)

Connot find module 'typescript'

![](Snipaste_2020-05-29_16-41-30.png)

虽然在全局安装了 `typescript` ，但是本地没有安装

files' list in config file 'tsconfig.json' is empty

![](Snipaste_2020-05-29_16-50-51.png)

​	解决： 没有配置 `tsconfig.json` 文件  `tsconfig.json` 文件的配置?

运行打包命令` webpack --config ./build/webpack.base.js --mode=development` 报错

 ![](Snipaste_2020-06-01_11-47-36.png)

解决： ![](Snipaste_2020-06-01_11-48-09.png)

extensions 没有配置 .ts .tsx     resolve ->extensions 的作用？？

nodejs 里面的流？

使用 `gulp` 作为项目工程化的流程管理工具

搭建开发服务器

### 中间件

​	不要管具体实现，关注业务，那跟插件有什么区别？

​	实现具体业务的中间件怎么实现？ 



gulp dev : 对于使用 ts tsx 

​	读取合并 所有 tsconfig

​	读取合并所有 webpackconfig

​	起服务器运行

****

​	上面的这些是我遇到的一些问题和疑惑，还没彻底解决，`2020/06/02`  经过两天的时间写了一个基础（0.0.1）的版本`webpack + gulp` ，实现了

+ `webpack` 不同环境的配置（基础、测试和生产环境）

+ `webpack` 对不同文件的处理
+ `webpack` 监听文件变更自动增量编译 
+ `gulp`作为流程管理
+ 对TS的支持

## 安装NodeJs

​	nodejs下载： http://nodejs.cn/download/

​	下载对应版本，windows安装完后会自动配好环境变量，命令行`node -v` 查看node版本，没有自动配环境变量需要手动配置。

![](Snipaste_2020-06-02_17-56-19.png)



## npm项目初始化

新建文件夹，初始化项目

```
mkdir webpack20200601 && cd webpack20200601
npm init -y
```

初始化后会生成`package.json` 文件，下面是本次版本配置：

```
{
  "name": "webpack20200601",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "private": true,
  "dependencies": {
    "@babel/runtime": "^7.10.2",
    "react": "^16.13.1",
    "react-dom": "^16.13.1",
    "ts-loader": "^7.0.5"
  },
  "devDependencies": {
    "@types/node": "^14.0.6",
    "@types/react": "^16.9.35",
    "@types/react-dom": "^16.9.8",
    "@types/webpack": "^4.41.16",
    "@types/webpack-dev-server": "^3.11.0",
    "babel-core": "^6.26.3",
    "babel-eslint": "^10.1.0",
    "babel-plugin-external-helpers": "^6.22.0",
    "babel-plugin-import": "^1.13.0",
    "babel-plugin-syntax-dynamic-import": "^6.18.0",
    "babel-plugin-transform-class-properties": "^6.24.1",
    "babel-plugin-transform-decorators-legacy": "^1.3.5",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-env": "^1.7.0",
    "babel-preset-react": "^6.24.1",
    "connect-history-api-fallback": "^1.6.0",
    "copy-webpack-plugin": "^6.0.1",
    "del": "^5.1.0",
    "eslint": "^7.1.0",
    "eslint-plugin-react": "^7.20.0",
    "express": "^4.17.1",
    "gulp": "^4.0.2",
    "gulp-imagemin": "^7.1.0",
    "gulp-sequence": "^1.0.0",
    "gulp-uglify": "^3.0.2",
    "gulp-util": "^3.0.8",
    "html-webpack-plugin": "^4.3.0",
    "http-proxy-middleware": "^1.0.4",
    "less": "^3.11.2",
    "opn": "^6.0.0",
    "postcss": "^7.0.31",
    "ts-node": "^8.10.2",
    "typescript": "^3.9.3",
    "webpack": "^4.43.0",
    "webpack-bundle-analyzer": "^3.8.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.11.0",
    "webpack-hot-middleware": "^2.25.0",
    "webpack-merge": "^4.2.2"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

使用 `npm i` 安装需要的依赖。

依赖都会安装在这个文件夹下面

![](Snipaste_2020-06-03_17-37-26.png)

## webpack搭建自动开发环境

webpack 官网 ： https://webpack.docschina.org/concepts/



### 基础搭建



### 使用开发服务器



## 使用Gulp作为流程管理



