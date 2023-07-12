---
title: value而reactive不需要
date: 2023-07-12 22:00:24
permalink: /pages/dfe475/
categories:
  - 前端
  - 笔记
tags:
  - 
author: 
  name: Ling HuanLiang
  link: https://github.com/lilling02
---
# ref定义的对象类型的响应式在脚本中取值需要.value而reactive不需要

## 1. ref定义的响应式不论是不是对象类型都需要使用.value来取值

````js
// 对象类型的
import { ref } from 'vue';

const state = ref({count: 0})
const addCount = () => {
    state.value.count++   // 可以看到需要使用.value来取值
}

// 非对象类型的
import { ref } from 'vue';
const count = ref(0)
const addCount = () => {
    count.value++   // 可以看到需要使用.value来取值
}
````

## 2. reactive定义的响应式对象类型不需要使用.value来取值

````js
// 对象类型的
import { reactive } from 'vue';
// const state1 = ref({count: 0})
const state = reactive({count: 0})
const addCount = () => {
    // state.value.count++
    state.count++  //可以看到如果使用reactive定义的响应式对象类型不需要使用.value来取值
}
````

## 原理

可以从打印出来的信息中发现 如果是使用ref来定义一个响应式对象,那么ref会调用react来帮助将对象变成响应式对象 然后把新的响应式对象放到Ref对象的value属性中

![](F:\HuanLiang Ling for University\JAVA_WEB\md_image\ref和reactive.png)
