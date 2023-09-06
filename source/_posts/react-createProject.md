---
title: 创建自己的react项目
date: 2020-07-10 15:40:31
tags: 
  - React
categories:
  - React
---

## 全局安装create-react-app

执行命令

```cmd
npm install -g create-react-app
```

## 创建项目安装依赖

create-react-app是一个react项目的脚手架，会自动创建一个简单的项目并下载依赖项

```cmd
create-react-app <项目名称>
```

cd进入项目文件夹，执行``npm start``启动项目

项目启动后，默认打开3000端口号，如果有占用会询问是否选择其他端口代替

## 安装cnpm

执行以下语句

```cmd
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

之后安装项目依赖会非常快
