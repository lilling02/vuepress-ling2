---
title: pinia使用体验
date: 2023-07-12 22:00:24
permalink: /pages/17059a/
categories:
  - 前端
  - 笔记
tags:
  - 
author: 
  name: Ling HuanLiang
  link: https://github.com/lilling02
---
# pinia使用体验(关于写法)

关于如果安装pinia 就不说了,直接去看[文档](https://pinia.vuejs.org/zh/getting-started.html).我们这里只聊代码部分.

pinia文档中是有这么一个例子:

```` js
import { ref, computed } from 'vue'
import { defineStore } from 'pinia'
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const doubleCount = computed(() => count.value * 2)
  function increment() {
    count.value++
  }
  return { count, doubleCount, increment}
})
````

而对于使用pinia有这样一个例子:

```` js
import {useCounterStore} from './stores/counter'
// pinia 使用
const CounterStore = useCounterStore() // 1.获取pinia对象
// 模板中
<button @click="CounterStore.increment">点击count++</button>
<div>{{CounterStore.count}}</div>
<div>{{ CounterStore.doubleCount }}</div>
````

> 我们可以看到,其实在pinia中写代码 和在vue的sfc中写脚本的方式基本上一模一样,可以说***基本没有什么心智负担***.

## 异步action

> 思想:action函数既支持同步也支持异步,和在组件中发送网络请求写法保持一致
> 步骤:
>
> 1. store中定义action
> 2. 组件中触发action

1- store中定义action

```javascript
const API_URL = 'http://geek.itheima.net/v1_0/channels'

export const useCounterStore = defineStore('counter', ()=>{
  // 数据
  const list = ref([])
  // 异步action
  const loadList = async ()=>{
    const res = await axios.get(API_URL)
    list.value = res.data.data.channels
  }

  return {
    list,
    loadList
  }
})
```

2- 组件中调用action

```vue
<script setup>
  import { useCounterStore } from '@/stores/counter'
  const counterStore = useCounterStore()
  // 调用异步action
  counterStore.loadList()
</script>

<template>
  <ul>
    <li v-for="item in counterStore.list" :key="item.id">{{ item.name }}</li>
  </ul>
</template>
```

## storeToRefs保持响应式解构
>
> 直接基于store进行解构赋值,响应式数据(state和getter)会丢失响应式特性,使用storeToRefs辅助保持响应式

```vue
<script setup>
  import { storeToRefs } from 'pinia'
  import { useCounterStore } from '@/stores/counter'
  const counterStore = useCounterStore()
  // 使用它storeToRefs包裹之后解构保持响应式
  const { count } = storeToRefs(counterStore)

  const { increment } = counterStore
  
</script>

<template>
  <button @click="increment">
    {{ count }}
  </button>
</template>
```

### pinia 直接结构赋值会导致响应式丢失

``` js
// 如果这么写会导致响应式丢失
const {count,doubleCount} = consterStore
```

而如果你想要既要结构赋值又要保存响应式

解决方案:

``` js
// 解决结构赋值导致的响应式丢失
const {count,doubleCount} = storeToRefs(consterStore)
```

> 原因:直接结构赋值得到的是值,而加上storeToRefs是得到一个响应式对象
> ***如果是方法的话***,可以直接结构赋值不用加storeToRefs
