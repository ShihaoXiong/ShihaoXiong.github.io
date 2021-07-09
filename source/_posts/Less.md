---
title: Less
date: 2021-07-09 15:36:56
tags: [Web, Css]
toc: true
categories: Note
---

Less 是一种动态样式语言，属于 css 预处理器的范畴，它扩展了 css 语言，增加了变量、mixin、函数等特性，使 css 更易维护和扩展。
Less 可以在客户端上运行，也可以借助 node.js 在服务端运行。

<!-- more -->

---

## Less 中的注释

- 以 `//` 开头的注释，不会被编译到 css 文件中
- 以 `/**/` 包裹到注释会被编译到 css 文件中

---

## 变量

<u>使用@来声明一个变量</u>

### 作为普通属性来使用

直接使用 `@param`

```less
@color: #ff0000;
* {
	background: @color;
}
```

### 作为选择器和属性名

`@{param}`

```less
/* 选择器 */
@selector: #container;
@{selector} {
	position: relative;
	...;
}

/* 属性名 */
@m: margin;
* {
	@{m}: 0;
}
```

### 作为 url

`@{url}`

### 变量的延迟加载

<u>在所有变量加载完成后才会执行使用变量的语句</u>

## Less 中的嵌套规则

如下结构：

```html
<div class="outer">
	<div class="inner"></div>
</div>
```

### 基本嵌套规则

```less
.outer {
	.inner {
		position: relative;
		...;
	}
}
```

### &的使用

```less
.outer {
	.inner {
		position: relative;
		... &:hover {
			...;
		}
	}
}
```

---

## Less 混合

混合就是将一系列属性从一个规则集引入到另一个规则集的方式

### 普通混合/不带输出的混合

```less
// 定义混合
.mixin {
	position: absolute;
	...;
}

// 使用
.outer {
	.inner {
		.mixin;
	}
	.inner2 {
		.mixin;
	}
}
```

### 带参数(且有默认值)的混合

```less
// 定义混合
.mixin(@width: 200px, @height: 200px, @color: #00ff00) {
	position: absolute;
	width: @width;
	height: @height;
	background: @color;
	...;
}

// 使用
.outer {
	.inner {
		.mixin(100px, 100px, #ff0000);
	}
	.inner2 {
		/* 使用默认值 */
		.mixin;
	}
}
```

### 命名参数

```less
// 定义混合
.mixin(@width: 200px, @height: 200px, @color: #00ff00) {
	position: absolute;
	width: @width;
	height: @height;
	background: @color;
	...;
}

// 使用
.outer {
	.inner {
		.mixin(100px, 100px, #ff0000);
	}
	.inner2 {
		/* 命名参数 */
		.mixin(@color: #0000ff);
	}
}
```

### 匹配模式

```less
// 定义混合
/* 定义同名混合，并且第一个参数为@_时，在使用混合时会自动调用第一个参数为@_的同名混合 */
.mixin(@_) {
	width: 0;
	height: 0;
	border-width: 40px;
	border-style: solid;
}
/* top/right/bottom/left为匹配标识符，需要放在第一个参数，可自定义 */
.mixin(top) {
	border-color: red, transparent, transparent, transparent;
}
.mixin(right) {
	border-color: transparent, red, transparent, transparent;
}
.mixin(bottom) {
	border-color: transparent, transparent, red, transparent;
}
.mixin(left) {
	border-color: transparent, transparent, transparent, red;
}

// 使用
.triangle {
	.mixin(top);
	// .mixin(right);
	// .mixin(bottom);
	// .mixin(left);
}
```

### arguments 变量

```less
.mixin(@w, @s, @c) {
	border: @arguments;
}

.container {
	.border(1px, solid, #000000);
}
```

---

## Less 运算

```less
.container {
	// 计算的一方带单位即可
	width: 100 + 100px;
}
```

---

## Less 继承

::继承不能带参数::
<u>性能比混合高，灵活度比混合低</u>
如下示例：

```html
<div class="outer">
	<div class="inner"></div>
	<div class="inner"></div>
</div>
```

如果使用以下混入

```less
.mixin {
	// 内容1
}

.inner {
	&:nth-child(1) {
		.mixin;
	}
	&:nth-child(2) {
		.mixin;
	}
}
```

编译结果为

```css
.inner:nth-child(1) {
	// 内容1
}
.inner:nth-child(2) {
	// 内容1
}

/* 以上结果会影响渲染性能，通过继承使得编译结果为以下格式 */
.inner:nth-child(1),
.inner:nth-child(2) {
	// 内容1
}
```

使用继承

```less
// 定义继承
.extend {
	position: absolute;
	...;
}

// 使用
.inner:entend(.extend) {
	...;
}
```

编译结果为

```css
.inner,
.inner:nth-child(1),
.inner:nth-child(2) {
	// 内容1
}
/* .inner被重复编译 */
```

修改继承写法

```less
// 使用
.inner {
	&:extend(.extend);

	/* 使用all修饰符可继承extend所有属性，包括hover等伪类 */
	&:extend(.extend all);
}
```

---

## Less 避免编译

使用 `~""` 包含不需要 less 编译的内容

```less
.container {
	width: ~'calc(100px + 100)';
}
```
