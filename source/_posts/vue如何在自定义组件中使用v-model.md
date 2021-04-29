---

title: vue如何在自定义组件中使用v-model
toc: 1
date: 2019-02-22 13:07:40
top_img: /img/vue.png
cover: /img/vue.png
tags:
- VUE
categories:
- VUE
---

![image](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2448181890,3036702771&fm=26&gp=0.jpg)

vue中有时候用一个子组件，然后子组件向父组件传值的时候必须绑定相对应事件，感觉有点麻烦。既然是从react到vue，那么就想我们要用到vue自己特有的双向绑定v-model来更解决这个问题，让代码更简洁清晰。不多说，下面直接上简单的示例。

<!--more-->

### 1.一般的父子组件传值
一般子组件要改变父组件的值都要通过传入一个方法来改变。(如下)

父组件：
```vue
<!--父组件-->
<template>
<div class="parent">
  <p>示例值：{{data}}</p>
  <Child @changeData="changeData" :parentData="data"></Child>
</div>
</template>
<script>
import Child from './Child.vue';
export default {
  data() {
    return {
      data: '11111'
    };
  },
  components: {
    Child
  },
  methods: {
    changeData(val) {
      this.data = val;
    }
  }
}
</script>
```
子组件：

```vue

<template>
<div class="child">
  <p>父亲传来的值{{parentData}}</p>
  <button @click="changeData">改变</button>
</div>
</template>
<script>
export default {
  props: {
    parentData: String
  },
  methods: {
    changeData() {
      this.$emit('changeData', '子组件改它了222222');
    }
  }
}
</script>
```

### 2.子组件用v-model
这种方法无需传入方法到子组件，更简洁，但是子组件中写法有所不同。

父组件：

```vue
<template>
  <div class="component-test">
    父组件的值：{{value}}
    <CC v-model="value"/>
  </div>
</template>
<script>
import CC from './component'
export default {
  components: {
    CC
  },
  data() {
    return {
      value: '123'
    };
  },
  created() {
  },
  methods: {
  }
};
</script>

<style lang="less" scoped>

</style>
```

子组件：（注意看代码中的注释）

```vue
<!--子组件-->
<template>
  <div class="component">
    <a-input :value="string" @change="change"></a-input>
  </div>
</template>
<script>
export default {
  props: {
    string: { // 这个string要与下面model中prop声明的变量名相同
      type: String
    }
  },
  data() {
    return {
    };
  },
  model: {
    prop: 'string', // 这里string变量非常关键，prop是你随意命名的作为用来绑定父组件中v-model绑定的值(即父组件的value)的一个变量。
    event: 'changestring' // event指的是需要更改string时需要触发的函数
  },
  created() {
  },
  methods: {
    change(e) {
      this.$emit('changestring', e.target.value); // 这句也非常关键，如果没有这句父组件中的value不会更新，这句就是通过改变string来更新双向绑定的父组件中的value字段。
    }
  }
};
</script>

<style lang="less" scoped>

</style>
```


两种不同的绑定传值，各有各的应用场景和优势缺点，希望多种方法可以给大家更多思路，让代码更简洁方便，谢谢！





