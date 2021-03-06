---
title: BFC_IFC_GFC_FFC
date: 2018-02-04 19:56:09
top_img: img/wallpaper.png
cover: img/279759ee3d6d55fbe2fea0a66f224f4a20a4dd72.png
tags:
- CSS
categories:
- CSS
aplayer: true
---
# BFC_IFC_GFC_FFC

### **什么是FC**

Formatting context(格式化上下文) 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

### **BFC**

#### 什么是BFC

BFC(Block Formatting contexts)，"块级格式上下文"。BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

#### **如何产生BFC**

- float的值不是none。
- position的值不是static或者relative。
- display的值是inline-block、table-cell、flex、table-caption或者inline-flex
- overflow的值不是visible。

#### BFC布局规则

- 内部的box都会按**垂直**方向进行排列
- Box垂直方向的间距由**margin**决定，**同一个**BFC中相邻的两个box间距会重叠。
- 容器上面的子元素不会影响外面的元素，反之也是如此。
- 计算BFC高度时，浮动元素的高度也会计算进去，并且和浮动区域不会重叠。

#### **BFC的作用**

- 利用BFC可以避免margin重叠，当属于同一个BFC时，相邻的两个box边距会方式重叠，可以对两个box分别设置BFC，例如加上一个div标签。
- BFC可以自适应两栏的布局，因为BFC的区域不会和浮动区域重叠，所以设置"overflow: hidden";让box的宽度自适应。
- BFC可以清楚浮动。当父元素没有高度，子元素进行浮动时会产生高度塌陷，此时给父节点激活BFC可以解决这个问题。

### **IFC**

IFC(Inline Formating contexts),"内联格式上下文"。IFC的line box(线框)高度由其行内元素中最高的实际高度计算而来。
**作用**：
水平居中：当一个块要在环境中水平局中时，设置其为inline-block则会在外参产生IFC，通过text-align使其水平居中。
垂直居中：创建一个IFC，用其中一个元素撑开父元素的高度，然后设置其vertical-align:middle，其他元素则可以在这个父元素中垂直居中。

### GFC

GFC(GridLayout Formatting contexts),"网格布局上下文",当一个元素display设置为grid时，此元素将会获取到一个独立的渲染区域，我们可以通过	在网格容器（grid container）上定义网格定义行和网格定义列为每一个项目定义位置和空间。
**作用：**
GFC和table相比，会有更加丰富的属性来控制列，控制对齐以及更为精细的渲染语义和控制。

### FFC

FFC（felx Formatting contexts),"自适应格式上下文"。display属性为flex或者inline-flex的元素将会生成自适应容器(flex container)，不过只有谷歌和火狐支持该属性，但是在移动端可以完全使用。
Flex Box由伸缩容器和伸缩项目组成，设置为flex的容器被渲染成一个块级元素，而设置成Inline-flex的容器则渲染成一个行级元素。FlexBox定义了伸缩容器内，伸缩项目的如何布局。