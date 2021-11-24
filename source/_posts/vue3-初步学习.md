---
title: vue3-初步学习
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-30 09:26:21
password:
summary:
tags: Vue
categories: Vue-初识
---

# Vue3-初步学习

## 六大亮点

+ Performance: 性能比vue2.x快1.2~2倍
+ Tree shaking support: 按需编译，体积比Vue2.x更小
+ Composition API: 组合API（类似React Hooks）
+ Better TypeScript support: 更好的Ts支持
+ Custom Render API: 暴露了自定义渲染API
+ Fragment，Teleport(Protal)，Suspense: 更先进的组件

## 性能如何提升

+ 新增静态标记(diff算法优化)

**vue2.x diff算法(以[snabbdom](https://github.com/snabbdom/snabbdom#opportunity-for-community-feedback)为例)**

```javascript
import {
  init,
  classModule,
  propsModule,
  styleModule,
  eventListenersModule,
  h,
} from "snabbdom";

const patch = init([
  // Init patch function with chosen modules
  classModule, // makes it easy to toggle classes
  propsModule, // for setting properties on DOM elements
  styleModule, // handles styling on elements with support for animations
  eventListenersModule, // attaches event listeners
]);

const container = document.getElementById("container");

const vnode = h("div#container.two.classes", { on: { click: someFn } }, [
  h("span", { style: { fontWeight: "bold" } }, "This is bold"),
  " and this is just normal text",
  h("a", { props: { href: "/foo" } }, "I'll take you places!"),
]);
// Patch into empty DOM element – this modifies the DOM as a side effect
// 将vnode 渲染到空的容器里面
patch(container, vnode);

const newVnode = h(
  "div#container.two.classes",
  { on: { click: anotherEventHandler } },
  [
    h(
      "span",
      { style: { fontWeight: "normal", fontStyle: "italic" } },
      "This is now italic type"
    ),
    " and this is still just normal text",
    h("a", { props: { href: "/bar" } }, "I'll take you places!"),
  ]
);
// Second `patch` invocation
// 更新vnode
patch(vnode, newVnode); // Snabbdom efficiently updates the old view to the new state

```

​	vnode：使用js来描述真实的dom节点

​	h函数：生成vnode

​				![](https://i.loli.net/2021/06/30/OYTRcAr9z1Vpn8o.png)

​			h函数最终返回的是`{ sel, data, children, text, elm, key }`集合对象

​				sel: 选择器， 类似于div、p	

​				data: 事件、样式等

​				children: 子节点（children与text只会存在一个）

​				text: 纯文本（children与text只会存在一个）

​				elm: 真实的dom元素

​				key: 唯一标志

​	patch函数：**首次渲染将vnode渲染到空的容器** 以及 **更新vnode**

```javascript
return function patch(oldVnode: VNode | Element, vnode: VNode): VNode {
    let i: number, elm: Node, parent: Node;
    const insertedVnodeQueue: VNodeQueue = [];
    for (i = 0; i < cbs.pre.length; ++i) cbs.pre[i]();
    // 如果oldVnode是dom，就去生成vnode，emptyNodeAt返回的是一个vnode
    if (!isVnode(oldVnode)) {
      oldVnode = emptyNodeAt(oldVnode);
    }
    // 如果两个是相同的vnode（比较vnode的sel和key），就去更新
    if (sameVnode(oldVnode, vnode)) {
      patchVnode(oldVnode, vnode, insertedVnodeQueue);
    } else {
      // 不是相同的vnode
      elm = oldVnode.elm!;
      parent = api.parentNode(elm) as Node;
      // 创建dom元素
      createElm(vnode, insertedVnodeQueue);

      if (parent !== null) {
        // 插入新的dom元素
        api.insertBefore(parent, vnode.elm!, api.nextSibling(elm));
        // 删除老的dom元素
        removeVnodes(parent, [oldVnode], 0, 0);
      }
    }

    for (i = 0; i < insertedVnodeQueue.length; ++i) {
      insertedVnodeQueue[i].data!.hook!.insert!(insertedVnodeQueue[i]);
    }
    for (i = 0; i < cbs.post.length; ++i) cbs.post[i]();
    return vnode;
  };
```

patchVnode：**当老的vnode和新的vnode相等时，调用更新**

```javascript
function patchVnode(
    oldVnode: VNode,
    vnode: VNode,
    insertedVnodeQueue: VNodeQueue
  ) {
    const hook = vnode.data?.hook;
    hook?.prepatch?.(oldVnode, vnode);
    // 给新的vnode关联dom元素
    const elm = (vnode.elm = oldVnode.elm)!;
    const oldCh = oldVnode.children as VNode[];
    const ch = vnode.children as VNode[];
    if (oldVnode === vnode) return;
    if (vnode.data !== undefined) {
      for (let i = 0; i < cbs.update.length; ++i)
        cbs.update[i](oldVnode, vnode);
      vnode.data.hook?.update?.(oldVnode, vnode);
    }
    // 如果新的vnode不存在文本节点
    if (isUndef(vnode.text)) {
      // 老的vnode存在children & 新的vnode存在存在children
      if (isDef(oldCh) && isDef(ch)) {
        // 两者的children不相等，进行更新
        if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue);
        // 新的vnode存在children，老的vnode不存在children
      } else if (isDef(ch)) {
        // 将elm的text设置为空
        if (isDef(oldVnode.text)) api.setTextContent(elm, "");
        // 更新elm，插入children
        addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue);
        // 新的vnode不存在children，老的vnode存在children
      } else if (isDef(oldCh)) {
        // 更新elm，移除老vnode的children
        removeVnodes(elm, oldCh, 0, oldCh.length - 1);
        // 两者都不存在children，新的vnode即不存在children 也不存在text，老的vnode存在text
      } else if (isDef(oldVnode.text)) {
        // 更新elm，设置text为空
        api.setTextContent(elm, "");
      }
      // 两者的text并不相等，可能为null
    } else if (oldVnode.text !== vnode.text) {
      // 如果老的vnode存在children则移除
      if (isDef(oldCh)) {
        removeVnodes(elm, oldCh, 0, oldCh.length - 1);
      }
      // 更新elm，设置text
      api.setTextContent(elm, vnode.text!);
    }
    hook?.postpatch?.(oldVnode, vnode);
  }
```

updateChildren：**当老的vnode的children和新的vnode的children不相等是调用，进行后代更新**

```javascript
function updateChildren(
    parentElm: Node,
    oldCh: VNode[],
    newCh: VNode[],
    insertedVnodeQueue: VNodeQueue
  ) {
    let oldStartIdx = 0;
    let newStartIdx = 0;
    let newEndIdx = newCh.length - 1;
    let oldEndIdx = oldCh.length - 1;
    let oldStartVnode = oldCh[0];
    let oldEndVnode = oldCh[oldEndIdx];
    let newStartVnode = newCh[0];
    let newEndVnode = newCh[newEndIdx];
    let oldKeyToIdx: KeyToIndexMap | undefined;
    let idxInOld: number;
    let elmToMove: VNode;
    let before: any;

    while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
      if (oldStartVnode == null) {
        oldStartVnode = oldCh[++oldStartIdx]; // Vnode might have been moved left
      } else if (oldEndVnode == null) {
        oldEndVnode = oldCh[--oldEndIdx];
      } else if (newStartVnode == null) {
        newStartVnode = newCh[++newStartIdx];
      } else if (newEndVnode == null) {
        newEndVnode = newCh[--newEndIdx];
      } else if (sameVnode(oldStartVnode, newStartVnode)) {
        patchVnode(oldStartVnode, newStartVnode, insertedVnodeQueue);
        oldStartVnode = oldCh[++oldStartIdx];
        newStartVnode = newCh[++newStartIdx];
      } else if (sameVnode(oldEndVnode, newEndVnode)) {
        patchVnode(oldEndVnode, newEndVnode, insertedVnodeQueue);
        oldEndVnode = oldCh[--oldEndIdx];
        newEndVnode = newCh[--newEndIdx];
      } else if (sameVnode(oldStartVnode, newEndVnode)) {
        // Vnode moved right
        patchVnode(oldStartVnode, newEndVnode, insertedVnodeQueue);
        api.insertBefore(
          parentElm,
          oldStartVnode.elm!,
          api.nextSibling(oldEndVnode.elm!)
        );
        oldStartVnode = oldCh[++oldStartIdx];
        newEndVnode = newCh[--newEndIdx];
      } else if (sameVnode(oldEndVnode, newStartVnode)) {
        // Vnode moved left
        patchVnode(oldEndVnode, newStartVnode, insertedVnodeQueue);
        api.insertBefore(parentElm, oldEndVnode.elm!, oldStartVnode.elm!);
        oldEndVnode = oldCh[--oldEndIdx];
        newStartVnode = newCh[++newStartIdx];
      } else {
        if (oldKeyToIdx === undefined) {
          oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx);
        }
        idxInOld = oldKeyToIdx[newStartVnode.key as string];
        if (isUndef(idxInOld)) {
          // New element
          api.insertBefore(
            parentElm,
            createElm(newStartVnode, insertedVnodeQueue),
            oldStartVnode.elm!
          );
        } else {
          elmToMove = oldCh[idxInOld];
          if (elmToMove.sel !== newStartVnode.sel) {
            api.insertBefore(
              parentElm,
              createElm(newStartVnode, insertedVnodeQueue),
              oldStartVnode.elm!
            );
          } else {
            patchVnode(elmToMove, newStartVnode, insertedVnodeQueue);
            oldCh[idxInOld] = undefined as any;
            api.insertBefore(parentElm, elmToMove.elm!, oldStartVnode.elm!);
          }
        }
        newStartVnode = newCh[++newStartIdx];
      }
    }
    if (oldStartIdx <= oldEndIdx || newStartIdx <= newEndIdx) {
      if (oldStartIdx > oldEndIdx) {
        before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].elm;
        addVnodes(
          parentElm,
          before,
          newCh,
          newStartIdx,
          newEndIdx,
          insertedVnodeQueue
        );
      } else {
        removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx);
      }
    }
  }
```

​	Vue2中虚拟dom是进行全量比较

​	Vue3新增静态标记(PatchFlag)，在于上次虚拟节点进行比较的时候，只对比带有patch falg的节点

+ 静态提升

  

+ 事件监听缓存

+ ssr渲染

