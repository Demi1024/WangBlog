---
title: React项目配置Typescript
date: 2020-07-02 21:48:29
tags:
- TypeScript
categories:
- React
---

## 安装全局环境

### (1) 安装

```javascript
npm i -g typescript
```

安装之后执行 ``tsc -v`` 查看版本号。

如果报错``tsc不是内部命令也不是外部命令``,说明全局安装失败。可以改用淘宝源进行安装

### (2)初始化配置文件

项目目录中执行

```javascript
tsc --init
```

在当前目录中创建一个tsconfig.json文件

### (3)修改配置文件

```json
{
    ...,
    "allowJs": true
    "jsx": "preserve",
    "experimentalDecorators": true,
    ...
}
```

参数一 ：allowJs，使用现有的js文件

参数二：jsx
TypeScript具有三种JSX模式：preserve，react和react-native。 这些模式只在代码生成阶段起作用 - 类型检查并不受影响。 在preserve模式下生成代码中会保留JSX以供后续的转换操作使用（比如：Babel）。 另外，输出文件会带有.jsx扩展名。 react模式会生成React.createElement，在使用前不需要再进行转换操作了，输出文件的扩展名为.js。 react-native相当于preserve，它也保留了所有的JSX，但是输出文件的扩展名是.js

参数三：experimentalDecorators，设置为true支持装饰器语法

## 安装开发环境依赖

```cmd
npm i --save-dev \
typescript \
tslint-loader \
@types/react @types/react-dom
```

@types/react和@types/react-dom，是react和react-dom的Declaration Files。

## 配置编译规则

react的项目是使用customize-cra,无需修改overrides.js文件即可使用

## 配置代码检查规则

使用eslint对检查 tsx 文件

代码编辑器是vscode,在首选项中开启自动修复

```js
    {
        "eslint.validate": [
            "javascript",
            "javascriptreact",
            "typescript",
            "typescriptreact"
        ]
    }
```

## 配置文件路径别名

直接在tsconfig.json去填写paths的话，在项目运行时会提示 ``compilerOptions.paths must not be set (aliased imports are not supported)`` 

并且会自动删除paths的配置语句

为了绕过这个自动删除语句，项目目录新建paths.json文件

```js
{
    "compilerOptions": {
        "baseUrl": "src",
        "paths": {
            "@/*": ["src/*"],
        }
    }
}
```

在根目录下的tsconfig.json新增配置信息

```json
{
  "extends": "./paths.json"
}
```

## 问题记录

### 1.引入js或第三方js库

在tsconfig.js的配置中增加如下配置

```json
{
  "include": [
    "./src",
    "./src/utils/*.js", // 对于自写的js文件
    "typings/**/*" // 对于第三方库
  ],
}
```

include指明的js路径，可以节省声明文件，在使用时，直接import即可

对于第三方的模块，有两种方法

一、下载支持ts的npm包

当我们要使用一个类库时，需要ts声明文件，对外暴露API，有时候声明文件在源码中，大部分是单独提供额外安装。比如react,react-dom需要额外安装类型声明包。

大部分的类库，TS社区都有声明文件。使用**@types**进行管理,名称为@types/类库名。[**搜索类库**](http://microsoft.github.io/TypeSearch/)

```js
cnpm install --save @types/node
```

二、在include中指定声明路径

如果想以下面的方式引入文件

```js
import * as picture from './image/picture.png'
```

typings文件夹下创建xx.d.ts声明文件,[**声明文件写法**](http://definitelytyped.org/guides/contributing.html)

```js
declare module 'guacamole-common-js'{
  export default Guacamole
}
declare module '*.scss' {
    const content: any;
    export = content;
}
declare module '*.jpeg' {
    const content: any;
    export = content;
}
declare module '*.jpg' {
    const content: any;
    export = content;
}
declare module '*.png' {
    const content: any;
    export = content;
}
declare module '*.svg' {
    const content: any;
    export = content;
}
declare module '*.gif' {
    const content: any;
    export = content;
}
declare module '*.css' {
    const content: any;
    export default content;
}
```

```js
export = content  // 定义为默认导出
export default content // 全部导出
```

然后在使用到的index.tsx文件中直接引入即可

```js
import Guacamole from 'guacamole-common-js';
```
