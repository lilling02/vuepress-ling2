---
title: 坑-vue有basecss 使得样式出错
date: 2023-07-12 22:00:24
permalink: /pages/e4dd1e/
categories:
  - 前端
  - 笔记
tags:
  - 
author: 
  name: Ling HuanLiang
  link: https://github.com/lilling02
---
# 记录那些坑人玩意

## 2023/5/11 第一坑

在vue的模板代码里面一直都会自动引入一个basecss,就是这个基础的css文件我们会经常忘记导致会出现各种各样的样式问题

##### main.js

```` js
import './assets/main.css' // 就是他 记得每次遇见他给删了

import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'
import router from './router'

// 引入初始化文件
import '@/styles/common.scss'

const app = createApp(App)

app.use(createPinia())
app.use(router)
app.mount('#app')
````
