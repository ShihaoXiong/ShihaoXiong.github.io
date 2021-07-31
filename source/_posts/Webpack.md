---
title: Webpack
date: 2021-07-28 09:52:40
tags:
---

## Webpack 介绍

Webpack 是一个现在的 JavaScript 应用的静态**模块打包**工具。
![](Webpack/%E7%85%A7%E7%89%87%202020%E5%B9%B48%E6%9C%8814%E6%97%A5%20113202.jpg)

**打包：**

1. 将 Webpack 中的各种资源模块进行打包合并成一个或多个包 (Bundle)；
2. 并且在打包的过程中，还可以对资源进行处理，比如压缩图片，将 scss 转 成 css，将 ES6 语法转成 ES5 语法，将 TypeScript 转成 JavaScript 等操作；

<!-- more -->

---

## Webpack 安装

- 全局安装 (指定版本号 3.6.0)
  `npm install webpack@3.6.3 -g`
- 局部安装
  `—-save-dev` 是开发是依赖，项目打包后不需要继续使用

```powershell
cd url
npm install webpack@3.6.0 —-save-dev
```

---

## Webpack 的使用

### Webpack 的起步

文件打包： `webpack entryURL outputURL`

**目录结构：**

> dist：用于存放之后打包的文件
> src：用于存放源文件
> index.js：项目入口文件
> index.html：；浏览器打开展示的首页
> package.json：通过 `npm init` 生成的，npm 包管理的文件

### Webpack 的配置

#### webpack.config.js 及 package.json

使用 `npm init` 进行初始化；
创建 `webpack.config.js` 配置文件，内容如下：

```javascript
const path = require('path');

module.export = {
	//入口：可以是字符串/数组/对象
	entry: './src/index.js',
	//出口：通常是一个对象，里面至少包含两个重要属性，path和filename
	output: {
	  path: path.resolve(_dirname, 'dist')	//动态获取路径
	  filename: 'bundle.js',
	  publicPath: 'dist/'
	}
}
```

通过以上配置，在终端运行 `webpack` 即可进行打包

**package.json 中定义启动：**
在 `package.json` 中的"scripts"关键字下进行配置：

```json
"scripts": {
	"build": "webpack"
}
```

通过以上配置，在终端运行 `npm run build` 即可进行打包

#### loader

**使用步骤：**

1. 通过 npm 安装需要使用的 loader；
2. 在 webpack.config.js 中的"modules"关键字下进行配置；
   [loader 的安装及使用](https://www.webpackjs.com/loaders/)

---

## 使用 Vue 的配置

1. 使用 npm 下载 `npm install vue —-save`
2. 引入 vue 并使用：

```javascript
import Vue from 'vue';

new Vue({
	el: '#app',
	data: { message: 'Hello Vue!' }
});
```

4. 重新打包，运行程序
   运行时若浏览器报如下错误：
   ![](Webpack/%E7%85%A7%E7%89%87%202020%E5%B9%B48%E6%9C%8817%E6%97%A5%20111239.jpg)
   则需要在 `webpack.config.js` 中添加如下配置指定 runtime-complier 版本：

```javascript
resolve: {
	alias: {
	  'vue$': 'vue/dist/vue.esm.js'
	}
}
```

---

## 插件 (plugin)

**plugin 介绍：**

1. plugin 通常用于对某个现有的架构进行扩展
2. webpack 中的插件就是对 webpack 现有功能的各种扩展，比如打包优化，文件压缩等

**loader 和 plugin 的区别：**

1. loader 主要用于转换某些类型的模块，是一个~转换器~
2. plugin 是插件，是对 webpack 本身的扩展，是一个~扩展器~

**plugin 的使用步骤：**

1. 通过 npm 安装需要使用的 plugins
2. 在 `webpack.config.js` 中的 plugins 中进行配置

### 添加版权的 plugin (BannerPlugin)

属于 webpack 自带插件
如下修改 `webpack.config.js`

```javascript
const webpack = require('webpack');

module.exports = {
	...
	plugins: [
	  new webpack.BannerPlugin('最终版权归...所有')
	]
}
```

### 打包 html 的 plugin (HtmlWebpackPlugin)

**作用：**

1. 可以自动生成 index.html 文件 (可以指定模板来生成)
2. 将打包的 js 文件，自动通过 script 标签插入到 body 中

**安装：**
`npm install html-webpack-plugin —-save-dev`

**使用：**

```javascript
const HtmlWebpackPlugin = require('HtmlWebpackPlugin');

module.exports = {
	...
	plugins: [
	  new HtmlWebpackPlugin({
		//指定模板
		template: 'index.html'
	  })
	]
}
```

::若 index.html 模板中 script 中的 src 路径有问题，需要删除之前自定义的 output 中的 pubilcPath 属性::

### js 压缩 plugin (uglifyjs)

**使用：**

```javascript
const UglifyJsPlugin = require('uglify-webpack-plugin');

module.exports = {
	...
	plugins: [
	  new UglifyJsPlugin()
	]
}
```

---

## 本地服务器搭建 (dev-server)

webpack 提供了一个可选的本地开发服务器，这个本地服务器基于 node.js 搭建，内部使用 express 框架，可以实现我们想要的让浏览器 自动刷新显示我们修改后的结果。

dev-server 也是作为 webpack 中的一个选项 ，选项本身可以设置以下属性：

- conteneBase：为哪一个文件夹提供本地服务，默认是根文件夹
- port：端口号，默认 8080 端口
- inline：页面实时刷新
- historyApiFallback：在 SP 页面中，依赖 HTML5 的 history 模式

`webpack.config.`文件配置如下：

```javascript
devServer: {
	contentBase: './dist',
	inline: true
}
```

**package.json 中定义启动：**
在 `package.json` 中的"scripts"关键字下进行配置：

```json
"scripts": {
	"dev": "webpack-dev-server"
	//—-open会在执行后自动打开浏览器
	"dev": "webpack-dev-server —-open"
}
```

通过以上配置，在终端运行 `npm run dev` 即可执行

## 配置文件的分离

为分离生产环境和开发环境，webpack.config.js 文件可分离为以下三个文件：

- base.config.js
- prod.config.js
- dev.config.js

修改 `package.json` 中"scripts"关键字相关属性

```json
"scripts": {
	"build": "webpack —-config url/prob.config.js",
	"dev": "webpack-dev-server —-open —-config url/dev.config.js"
}
```

### 文件后缀省略

```javascript
//base.config.js
resolve: {
	extensions: ['.js', '.vue', '.json'];
}
```

通过以上设置，在导入文件时，可省略文件后缀名

### 文件夹别名

```javascript
//base.config.js
resolve: {
	alias: {
	  '@': resolve('src')
	}
}
```

通过以上设置，在填写 url 时，可直接使用"@"代替 src 文件的路径
