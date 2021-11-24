---
title: 好用的API-IntersectionObserver
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-21 14:41:34
password:
summary: IntersectionObserver真香
tags: 无限循环 惰性加载
categories: 好用的API
---

# 好用的API-IntersectionObserver

>​	前段时间研究PDF展示的时候，在github上找到了一段写的很好的代码（向大佬致敬），它实现加载更多的时候使用了[IntersectionObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)这个API，我觉得挺好的，分享给大家，然后写了一个[demo](https://github.com/muyeyong/test/tree/main/intersection-demo)，实现了惰性加载和无限滚动。

​	如果你不熟悉可以看看[这篇文章](https://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html)，里面的内容可能没有及时更新，可搭配[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)食用。先看一下demo效果：

​		![](https://i.loli.net/2021/05/21/XoALV34m7zeaMYK.gif)

​	动图太快，我们看静态.png

![](https://i.loli.net/2021/05/21/IGifLM7ZNVD6wnH.png)

​	最开始渲染了，**11**个`li`标签，但是只有在屏幕上显示的**5**个`li`标签才加载了图片，其他在不可见范围内的`li`是没有加载图片的，实现了`资源的懒加载`，在滚动的过程中，会加载可视范围内`li`的图片资源，滚动到底后，会加载更多图片，实现`无限滚动`。

![](https://i.loli.net/2021/05/21/mYSUk7qElPZXOoF.png)

​	这一切的一切全是全部是`IntersectionObserver`的功劳。

```js
// APP.vue
<template>
  <div id="app">
    <ul class="imgs">
      <li v-for=" i in wrapMaxCount" :key="i" v-visible.once="loadImg" class="img-item" ></li>
    </ul>
    <div  v-visible = "loadMore" class="observer"></div>
  </div>
</template>

<script>
import visible from './directives/visible'

export default {
  name: "App",
  components: {},
  directives: {
    visible,
  },
  data(){
    return {
      maxCount: 11,
      currIndex: 0,
      wrapMaxCount: 11
    }
  },
  mounted() {
    this.getImg()
  },
  methods: {
    getImg(i){
     if(typeof i === 'number')
      return require(`./assets/demo${i}.jpg`)
    },
    loadImg(e){
      const index = this.currIndex % this.maxCount
      const elImg = document.createElement('img')
      elImg.src =  this.getImg(index)
      e.appendChild(elImg)
      this.currIndex += 1
    },
    loadMore(){
      console.log('loadMore')
     this.wrapMaxCount += 5
    },
    fetch(){

    },
    smartNav(elements, options) {
      var defaults = {
        nav: null,
      };
      var params = Object.assign({}, defaults, options || {});

      if (typeof elements == "string") {
        elements = document.querySelectorAll(elements);
      }
      if (!elements.forEach) {
        return;
      }
      // 导航元素创建，如果没有
      if (!params.nav) {
        params.nav = document.createElement("div");
        params.nav.className = "title-nav-ul";
        document.body.appendChild(params.nav);
      }

      var lastScrollTop = document.scrollingElement.scrollTop;

      var zxxObserver = new IntersectionObserver(function (entries) {
        if (params.isAvoid) {
          return;
        }
        entries.reverse().forEach(function (entry) {
          if (entry.isIntersecting) {
            entry.target.active();
          } else if (entry.target.isActived) {
            entry.target.unactive();
          }
        });

        lastScrollTop = document.scrollingElement.scrollTop;
      });

      elements.forEach(function (ele, index) {
        var id = ele.id || ("smartNav" + Math.random()).replace("0.", "");
        ele.id = id;
        // 导航元素创建
        var eleNav = document.createElement("a");
        eleNav.href = "#" + id;
        eleNav.className = "title-nav-li";
        eleNav.innerHTML = ele.textContent;
        params.nav.appendChild(eleNav);
        ele.active = function () {
          // 对应的导航元素高亮
          eleNav.parentElement
            .querySelectorAll(".active")
            .forEach(function (eleActive) {
              ele.isActived = false;
              eleActive.classList.remove("active");
            });
          eleNav.classList.add("active");
          ele.isActived = true;
        };
        ele.unactive = function () {
          // 对应的导航元素高亮
          if (document.scrollingElement.scrollTop > lastScrollTop) {
            elements[index + 1] && elements[index + 1].active();
          } else {
            elements[index - 1] && elements[index - 1].active();
          }
          eleNav.classList.remove("active");
          ele.isActived = false;
        };

        // 观察元素
        zxxObserver.observe(ele);
      });

      params.nav.addEventListener("click", function (event) {
        var eleLink = event.target.closest("a");
        // 导航对应的标题元素
        var eleTarget =
          eleLink && document.querySelector(eleLink.getAttribute("href"));
        if (eleTarget) {
          event.preventDefault();
          // Safari不支持平滑滚动
          eleTarget.scrollIntoView({
            behavior: "smooth",
            block: "center",
          });

          if (CSS.supports("scroll-behavior: smooth")) {
            params.isAvoid = true;
            setTimeout(function () {
              eleTarget.active();
              params.isAvoid = false;
            }, Math.abs(
              eleTarget.getBoundingClientRect().top - window.innerHeight / 2
            ) / 2);
          } else {
            eleTarget.active();
          }
        }
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.imgs{
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1rem auto;
}
.img-item{
  width: 31.25rem;
  height: 31.25rem;
  list-style: none;
}
.img-item img{
  width: 100%;
  height: 100%;
}


</style>

```

```javascript
// directive visible
const observerMap = new WeakMap()

const createObserve = (el,vnode,modifiers,callBack)=>{
    const observer = new IntersectionObserver(entries=>{
        const entry = entries[0]
        if(entry.isIntersecting){
            callBack(el)
            if( modifiers && modifiers.once){
                
                disconnectObserver(observer)
            }
        }
    })
    vnode.context.$nextTick(()=>observer.observe(el))
    return observer
}

const disconnectObserver = (observer)=>{
  if(observer){
    observer.disconnect()
  }
}

const bind = (el,{value,modifiers },vnode) => {
    if (typeof window.IntersectionObserver === 'undefined') {
        throw('IntersectionObserver API is not available in your browser.')
      }else{
        const observer = createObserve(el,vnode,modifiers,(el)=>{
            const callback = value
            if(typeof callback === 'function')
            callback(el)
        })
        observerMap.set(el,{observer})
      }
}

const update = (el, { value, oldValue,modifiers }, vnode) =>{
    if(value === oldValue) return
    const {observer} = observerMap.get(el)
    disconnectObserver(observer)
    bind(el,{value,modifiers},vnode)
}

const unbind = (el)=>{
    if(observerMap.has(el)){
        const {observer} = observerMap.get(el)
        disconnectObserver(observer)
        observerMap.delete(el)
    }
}

export default {
    bind,
    update,
    unbind
}
```

## 实现

​	懒加载：

​		`IntersectionObserver`提供`isIntersecting`方法，检测元素是否出现在可视区域内，如果出现在可视区域内就去执行回调函数。once修饰符确保只执行一次回调，update方法中的 ` if(value === oldValue) return`判断是为了保证已经渲染过的元素不在重新渲染，`value`是你绑定的值，也是需要回调的函数，不要使用`()=>doSome()`，会被判定成不相等，会重复渲染。

无限加载：

​	放一个空白的`div`在底部，当出现的时候去执行相应的加载回调函数。		

​	