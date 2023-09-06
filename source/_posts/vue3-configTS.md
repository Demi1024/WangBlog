---
title: VUE3.0 造轮子之-项目TypeScript配置
date: 2020-07-29 21:11:09
tags: 
  - TypeScript
  - Vue3
categories:
  - Vue造轮子
---

本项目基于vite构建的vue3.0项目

## 一、初始化项目

创建项目有两种方式

官方文档中写的是

```cmd
npm init vite-app <项目名>
或者
yarn create vite-app <项目名>
```

而我创建项目使用的是以下的方法

首先，通过命令行全局安装create-vite-app

```cmd
npm i -g create-vite-app@1.18.0
或
yarn global add create-vite-app@1.18.0
```

安装完成后，可以使用`create-vite-app`或者`cva`来创建项目

项目创建完成后cd进入目录，先执行install安装依赖

之后就可以`run dev`在本地运行项目了

打开链接看到下图说明项目已经成功运行了
![vite搭建完成的页面](http://ww1.sinaimg.cn/large/0063Qlsmgy1gh86owbcygj30q00gcgm3.jpg)

## 二、声明vue文件

首先我们将主入口文件main.js的后缀名修改为ts

然后编辑器会有报错提示，因为TypeScript只能理解ts的文件，此时我们引入的.vue文件在ts中无法解析

解决此问题，需要新增一个xx.d.ts的声明文件

内容写入

```js
// 增加对vue文件的声明
declare module "*.vue" {
  import {ComponentOptions} from 'vue'
  const componentOptions:ComponentOptions
  export default componentOptions
}
```

增加的d.ts类型为文件时告诉ts如何理解.vue的文件
