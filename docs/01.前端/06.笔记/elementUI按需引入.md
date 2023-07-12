---
title: elementUI按需引入
date: 2023-07-12 22:00:24
permalink: /pages/c3511b/
categories:
  - 前端
  - 笔记
tags:
  - 
author: 
  name: Ling HuanLiang
  link: https://github.com/lilling02
---
 # 我经常看了文档还是不会引入elementUI所以记录一下

## 1. 先引入elementUI的完全体

``` 
# 选择一个你喜欢的包管理器

# NPM
$ npm install element-plus --save

# Yarn
$ yarn add element-plus

# pnpm
$ pnpm install element-plus
```

## 2.按需引入

### 自动导入**推荐**

首先你需要安装`unplugin-vue-components` 和 `unplugin-auto-import`这两款插件

```` 
npm install -D unplugin-vue-components unplugin-auto-import
````

然后把下列代码插入到你的 `Vite` 或 `Webpack` 的配置文件中

#### Vite

```
// vite.config.ts
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
  // ...
  plugins: [
    // ...
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
})
```

## 然后随意去找一个组件测试一下就可以了

