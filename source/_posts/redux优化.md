---

title: redux使用优化
toc: 1
date: 2020-03-01 16:57:49
top_img: /img/bg2016091801.png  
cover: /img/13142323.png
tags:
- react
categories:
- react
---

![image](https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1858669959,1889442191&fm=26&gp=0.jpg)

* 大家在使用redux的时候有没有发现真的，要定义不同的actiontype，写各种情况的reducer真的很麻烦，本次优化的目的就是想让redux能够达到跟this.setState一样简单的使用，看起来易懂让人知道你改变了哪些值，也便于后期多人维护。

<!--more-->

# Redux的使用优化

本文主要是因为当前数栈等项目中，由于使用redux的时候大家的使用方式有所不同，且部分使用不规范。导致后面其他人来接手维护或者开发新的代码的时候非常耗去寻找对应的状态，和状态改变的过程。所以提出一个对redux的优化和便捷使用的方式，能更方便维护和开发。实现该目的的核心文件用[此链接可以下载](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/merge.ts),该文件是ts文件格式。

### 1.简介

redux优化前后使用的对比如下：

1. 新增redux的store中状态的时候：
    * 优化前，由于后期需求的增加可能需要在redux的store中增加状态，我们一般需要在对应的actiontype文件中增加新的actiontype，在reducer中的initialState中增加新的字段，并且还需要增加对应的actiontype更改的case。
    * 优化后，我们就可以像state对象加字段一样，只在initialState中增加需要的新增字段即可，无需去声明actiontype等其他操作(类似于state的使用)。
2. 维护寻找redux的store中对应的状态改变的时候：
    * 优化前，我们在actions文件中看到的是一个又一个的dispath根据对应的actiontype去传入需要改变的payload，可能对于开发者来说知道他去改变的是store中的哪个状态，但是对于其他人可能需要一层又一层的去找这个actiontype对应的是redux状态中的哪个字段，非常的耗时，且有些时候把对应的actiontype的状态改变写在case中，让人难以发现。如下图两种情况：
        1. 在reducer中这种写法对后期维护不是很友好
        ![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/wrongReducer.jpg?raw=true)
        2. 在aciton中非常多的dispatch，我们并不知道到底对应改变的store的的哪些状态字段，需要去reducer中寻找
        ![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/wrongAction.jpg?raw=true)
    * 优化后，我们有个update函数(他的使用类似于setState函数的使用方法)，更便捷更改store中的状态且可以看到我们改变的是store中的那些状态字段，如下图：
    ![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/action.jpg?raw=true)

### 2.优化actiontype声明部分

与之前不同，之前是每个store中的状态都需要去声明对应的一个actiontype。现在只需要声明一个当前对应文件的UPDATE状态即可(声明方法可以用mirror-creator或者如图的方法也行)。
![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/actiontype.jpg?raw=true)

### 3.优化reducer部分

1. switch...case部分我只需要判断一个UPDATE的actiontype，内部则是封装了一个mergedeep函数。
![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/reducer.jpg?raw=true)
2. mergedeep函数：这是本次优化的关键，大致意思我们可以把它理解成跟this.setState的机制类似，去替换state中对应的传入需要改变的状态，其他的状态保持不变，这样一来，在后期需要新增字段时，无需再去actiontype中声明，也无需增加下面case的情况。( 新ts版本中mergedeep做了ts语法适配，已经放在如下src的utils文件当中)
![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/merge.jpg?raw=true)
3. mergedeep函数中的_isMergeAtom字段用途：主要便于我们对redux中对象类型的状态更新使用。分为两种(如下图有范例)：
    1. 更新状态时加上_isMergeAtom可以跟this.state一样对整个对象重新赋值，不保留原有对象中的其他属性。
    2. 更新状态时不加_isMergeAtom则会只更新该对象下需要更新的属性，其他属性还是保持原有值不变。
![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/mergeAtom.jpg?raw=true)

### 4.优化action部分

这里我们的主要优化部分就是dispatch了，由于我们之前已经将所有的redux中的状态通过一个UPATE的actiontype去管理，所以在这里我们就可以将redux的dispatc模仿成setState的写法来使用，让开发和后期维护的人看起来都更清晰便捷。如下图：
![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/action2.jpg?raw=true)

### 5.view部分使用举例

这里很简单，就是直接在view页面，有时候有部分需要简单的改变store中的状态的之后就可以用起来跟setstate一样，非常方便，且利于后期维护的人知道你调用update函数是是去改变store中的哪些状态。

例如下图：是一个受控的Input组件，他的值是取至redux中的searchTxt状态，这时候onChange函数是需要去改变redux中的searchTxt状态的值，我们这时候就可以跟使用setState一样去使用redux的update函数去更新redux中的searchTxt状态。这样比以前更加便捷，而且易读。
![can't get data](https://github.com/runuowbd123/homework/blob/master/reduxOptimize/view.jpg?raw=true)
