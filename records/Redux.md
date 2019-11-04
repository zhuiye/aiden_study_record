### Redux

react 本身拥有管理state 的功能,也就是利用组件的生命周期函数,Redux 的定位是更好的**辅助管理**

### 什么情况下使用

1. react 解决不了的情况下再使用

2. 多个的组件需要用到全局的state
3. state 可预测,维护性,扩展性
4. 时间旅行,撤销更容易实现



## immutable

```ts
ReturnType(typeof fn)
```



https://dev.to/busypeoples/notes-on-typescript-returntype-3m5a



## webAssemble

https://zhuanlan.zhihu.com/p/79792515?utm_source=qq&utm_medium=social&utm_oi=665865873957588992

## SSR

https://www.zhihu.com/question/308792091



## Proxy

https://www.w3cplus.com/javascript/use-cases-for-es6-proxies.html

## MVC

https://zhuanlan.zhihu.com/p/27302766

https://github.com/livoras/blog/issues/11

## EFE百度

https://efe.baidu.com/

## MVVC

https://zhuanlan.zhihu.com/p/24475845?refer=mirone

## virtual DOM

起源:https://github.com/MrErHu/blog/issues/26

https://github.com/livoras/blog/issues/13

https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/



有赞

<https://tech.youzan.com/tag/front-end/>

## shop

<https://github.com/Polymer/shop>

资源整理

<https://apps.evozi.com/apk-downloader/?id=org.telegram.messenger>



## 虚拟化长列表

如果你的应用渲染了长列表（上百甚至上千的数据），我们推荐使用“虚拟滚动”技术。这项技术会在有限的时间内仅渲染有限的内容，并奇迹般地降低重新渲染组件消耗的时间，以及创建 DOM 节点的数量。

[react-window](https://react-window.now.sh/) 和 [react-virtualized](https://bvaughn.github.io/react-virtualized/) 是热门的虚拟滚动库。 它们提供了多种可复用的组件，用于展示列表、网格和表格数据。 如果你想要一些针对你的应用做定制优化，你也可以创建你自己的虚拟滚动组件，就像 [Twitter 所做的](https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3)。



https://zhuanlan.zhihu.com/p/54327805