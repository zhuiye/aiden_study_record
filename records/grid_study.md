# Grid布局学习

参考   :https://www.css88.com/archives/8510#prop-grid-column-row

 二维基于网格的布局

基本现在的浏览器都支持

## 一些定义

网格容器(Grid Container) 应用display:grid;的元素

网格项(Grid Item):网格容器的子项

网格线(Grid LIne):构成网格结构的分界线

网格轨道(Grid Track):两条相邻网格线之间的空间 ,你可以把它们想象成网格的列或行.

网格单元格(Grid Cell):两个相邻的行和两个相邻的列网格线之间的空间。

网格区域(Grid Area):4条网格线包围的总空间。一个 网格区域(Grid Area) 可以由任意数量的 网格单元格(Grid Cell) 组成

## Grid Container

给一个元素应用网格系统

```css
.container {
    display:grid|inline-grid; /**grid 块状网格容器，inline-grid 内联网格**/
}
```

## grid-template-columns 

该属性定义网格系统中的有多少列，或者列的大小.值可以是百分比，长度值或者等份网格容器中可用空间（使用 `fr` 单位）

例子:

```css
.container {
  grid-template-columns:60px 60px auto 60px  /*定义了四列,其中三列60px,另一列自动填充父元素的剩余空间*/
}
```

## grid-template-rows

改属性的值的类型同 columns 大致

```css
.container {
  grid-template-rows: 60px 60px;/*定义了两行，行高度都为60px */
}
```

