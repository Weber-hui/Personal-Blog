---

title: flex布局
toc: 1
date: 2018-11-01 11:35:29
top_img: /img/bg2015071002.png
cover: /img/bg2015071002.png
tags:
- css
categories:
- css

---

![image](http://sttroke.com/wp-content/uploads/2017/08/flexbox.jpg)

* 经历了一个多月的毕设制作以及毕业事宜，今天又回到了程序员的岗位上，发现对flex布局有点小忘却，那就在阮一峰大哥的帮助下再次回顾温习flex布局的用法吧！

<!--more-->

flex布局是在09年由w3c提出的一种新方案，所以他的兼容性要特别注意对老的浏览器不兼容。兼容性如下：
![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071003.jpg)

### Flex的使用

任何一个容器都可以用Flex布局，当浏览器是Webkit内核的时候必须加上-webkit前缀。
**并且注意点：1、flex属性是设置在父级元素上的。2、当flex之后，float、clear、vertical-align属性都会失效**

```css

.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}

```

### flex-direction属性

flex-direction属性决定主轴的方向（即子元素的排列方向）

```css
.box {
  flex-direction: column-reverse | column | row | row-reverse;
}
//下图为实现效果顺序：竖排起点在下 | 竖排起点在上 | 横排起点在左 | 横排起点在右
```
![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)

### flex-wrap属性

默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。


```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
//默认为不换行，上面分别为不换行，换行第一行在上面，换行第一行在下面

```

### 父元素属性 flex-flow

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。


```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}

```

### 父元素属性 justify-content

justify-content属性定义了项目在主轴上的对齐方式。

```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
上面分别为：左对齐，右对齐，居中，
```
![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

### 父元素属性 align-items

align-items属性定义项目在交叉轴上如何对齐。

```css

.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}

```

![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

### 父元素属性 align-content

align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。


```css

.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}

```

![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071012.png)

### 子元素属性 order
order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

```css
.item {
  order: <integer>;
}
```
![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071013.png)

### 子元素属性 flex-grow

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

### 子元素属性 flex-shrink
flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
```css
.item {
  flex-shrink: <number>; /* default 1 */
}

```
![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071015.jpg)

### 子元素属性 flex-basis
flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。**它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。**

```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

### 子元素属性 flex
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

### 子元素属性 align-self
align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![image](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071016.png)


