https://immerjs.github.io/immer/docs/introduction

不可变数据类型

## 前言

在 react 中

https://zhuanlan.zhihu.com/p/122187278

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify

如何进行自我突破
重新架构,重新划分
分模块
如何才变得如此优雅?

有必要 搞一章前端工程化

一个有意思的 检测 组件再次渲染渲染库

https://medium.com/welldone-software/why-did-you-render-mr-big-pure-react-component-2a36dd86996f
@welldone-software/why-did-you-render

react 性能优化之组件重新渲染 纯组件, 要避免不必要的渲染
https://medium.com/welldone-software/why-did-you-render-mr-big-pure-react-component-part-2-common-fixing-scenarios-667bfdec2e0f

Why is --isolatedModules error fixed by any import?

究其原因,是 typescript 对待文件的方式不同,在没有导入或者导出的情况下,ts 会将改文件识别为旧模式文件,该文件定义的 东西,会合并到全局的命名空间的 ,
--isolatedModules 是强制你是用文件模块模式
https://stackoverflow.com/questions/56577201/why-is-isolatedmodules-error-fixed-by-any-import/56577324
