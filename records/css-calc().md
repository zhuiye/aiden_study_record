## css calc() 计算属性

最近发现一个好用的 css 属性 也就是 calc ,能够动态的计算出属性的值,对于某些场景来说,非常的有用.

比如:我们一般在底部固定一个 tabNav,高度假设定为 64px;我们可以计算出页面出 tabNav 的可视区域高度为
calc(100vh - 64px)

需要注意的点,运算符号一定要用空格隔开

## 比如计算最小的高度的时候...

min-height:calc(100vh - 64px)

## 参考链接

- https://developer.mozilla.org/zh-CN/docs/Web/CSS/calc
