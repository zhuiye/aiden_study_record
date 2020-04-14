# React 中的 state 管理

## 前言

在 React 编程中,初学之初,我们会把变化的 state 存于类组件中 this.state 中,如下

```ts
  class Test extends React.Component {
    this.state={
         ...
    }
  }
```

接触 hook 之后,我们可以在函数组件存储关于类的 state,如下:

```ts
function Test() {
  const [state, setState] = useState();
}
```

接着,我们可以利用 Redux 中管理我们的 state,通过 dispatch,这种唯一的方式,来更改我们的 state,部分代码类式如下:

```js
function reducer(state = {}, action) {
  switch (action.type) {
    case "--":
      return { ...state, ...action.value };
    case "++":
      return { ...state, ...action.value };
    default:
      return {};
  }
}
// 更改store
dispatch({ type: "--", value: {} });
dispatch({ type: "++", value: {} });
```

## 组件的状态交由 react 管理

先说类组件,改变 state 的状态只能是通过 this.setState(),这是硬性规定,对于不复杂的组件,或者页面来说,状态交由 react 来管理已经足够了,而且,当组件卸载时,状态也随之抛弃,而 hook 的出现也让函数组件有了管理 state 的能力,当然,还有解决了 state 复用的问题,至于组件的 state 的在组件树种深层次传递的问题,我们可以用 context 进行解决

## 何时使用 Redux

Redux 并没有特别神奇的,原理大致如下:

通过 **createStore()**生成一个 store 对象,里面存着我们 App 的 state(**私有变量,通过 getState 获取**),以及更新 state 的**dispatch**函数.

如何配合 React 使用, react-redux 这个库解决了这个问题,原理大致如下:

1. context 从树的顶端传递 store 的 state ,以及 dispatch 函数,
2. 对于需要的组件,或者页面,利用其提供的 connect 函数,从而获取所需的 state,以及 dispatch
3. dispatch(action)之后,react-redux 会自动处理那部分需要重新渲染

文档也说了,能用 react 解决的,就不需要强制用到 react,那么也说说 redux 到底解决了什么场景

### 实现数据缓存

利用 redux+react-redux,实现数据全局缓存,就算组件卸载,下次重新挂载的时候,也能会恢复状态

### 可预测,维护性,扩展性 时间旅行,撤销更容易实现

state 可预测,维护性,扩展性,对于每个 state 的变化,都有迹可循,对于一些 state 繁多的场景,我们可以有序的进行管理.**就好比一个灵活的 APP 配置模板页面,里面大致有轮播图模块,按钮链接模块,轮播的通知模块,商品配置模块...**,里面涉及的增删改查何其之多,还涉及深层处的组件树传值,此时就有了 redux 用武之地

## 究竟是该用 redux 还是由 React 来管理

轻度使用 redux,大部分的 state 还是交由 react

<!-- ### Redux

react 本身拥有管理 state 的功能,也就是利用组件的生命周期函数,Redux 的定位是更好的**辅助管理**

### 什么情况下使用

1. react 解决不了的情况下再使用

2. 多个的组件需要用到全局的 state
3. state 可预测,维护性,扩展性
4. 时间旅行,撤销更容易实现

## immutable

```ts
ReturnType(typeof fn);
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

## EFE 百度

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

https://zhuanlan.zhihu.com/p/54327805 -->
