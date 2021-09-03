---
title: ES6
date: 2021-07-09 15:53:35
tags: [Web, JavaScript]
toc: true
categories: Notes
---

## ECMAScript 和 JavaScript

- ECMA 是标准，JS 是实现
- ECMAScript 简称**ECMA**或**ES**

<!-- more -->

### 兼容性

## ES6 (ES 2015) — IE10+, Chrome, FireFox, Mobile, NodeJS

## 变量

- **var**
  1.  可以重复声明；
  2.  无法限制修改；
  3.  没有块级作用域；
- **let**
  1.  不能重复声明；
  2.  变量 — 可以修改；
  3.  块级作用域；
- **const**
  1.  不能重复声明；
  2.  常量 — 不能修改；
  3.  块级作用域；

---

## 函数

### 箭头函数

```javascript
function func() {
	//函数体
}

//箭头函数形式
() => {
	//函数体
};
```

1. 如果只有一个参数，`()`可以省略；
2. 如果只有一个 return，`{}`可以省略；

::箭头函数 中的 this 引用的是最近作用域的对象::

### 参数

1. **参数扩展/数组展开**
   1. 参数扩展；
      `function func(a,b,...参数名) {}` \* Rest Parameter 必须是最后一个
   2. 数组展开；
      `...arr` \* 展开后的结果和直接列出数组效果一样

```javascript
//示例
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

let arr = [...arr1, ...arr2];
//以上结果为arr = [1,2,3,4,5,6]
```

2. **默认参数**

```javascript
//默认b=3, c=5
function func(a, b = 3, c = 5) {
	//函数体
}

//传入参数后覆盖默认参数
func(2, 5, 7);
```

---

## 解构赋值

1. 左右两边结构必须一样；
2. 右边必须是合理的数据类型；
3. 声明和赋值必须在一句话里完成；

```javascript
//array
let [a, b, c] = [1, 2, 3];

//json
let { a, b, c } = { a: 1, b: 2, c: 3 };
```

---

## 数组

**新方法**

1. [.map()](https://www.jianshu.com/p/53032fc0909a) 映射
2. .reduce() 汇总

```javascript
//.reduce(callBackFun(previousValue,currentValue,index),initValue)

let arr = [...];

//求arr中总和
let result = arr.reduce(function(tmp, item, index) {
  return tmp + item;
});

//求arr平均数
let result = arr.reduce(function(tmp, item, index) {
  if(index!=arr.length-1) {
    return tmp + item;
  } else {
    return (tmp + item)/arr.length;
  }
});
```

3. .filter() 过滤器

```javascript
//filter的回调函数必须返回布尔值
//当返回为true时，会自动将这次回调 的item加到新的数组中
//当返回为 false时，函数内部会过滤掉这个item
let arr = [12, 5, 8, 99, 27, 36, 75, 11];

let result = arr.filter(item => {
	return item % 3 == 0;
});

//12,99,27,36,75
alter(result);
```

4. .forEach() 循环 (迭代)

---

## 字符串

**新方法**

1. .startsWith()
2. .endsWith()

```javascript
let str = “abcdefg”;
//true
str.startsWith(‘a’);
```

**字符串模板**

- 可用于字符串拼接
- 使用反单引号
- 可以折行

```javascript
let a = “b”;
//str = “abcdefg”;
let str = `a${a}cdeft`;
```

---

## 面向对象

### 老版本面向对象

```javascript
function User(name, pass) {
  this.name = name;
  this.pass = pass;
}
User.prototype.showName = function() {
  alert(this.name);
};
User.prototype.showPasss = function() {
  alert(this.pass);
};

var u1 = new User(‘admin’,’123456’);
u1.showName();
u1.showPass();

//继承
function VipUser(name,pass,level) {
  User.call(this,name,pass);

  this.level = level;
}
VipuUser.prototype = new User();
VipuUser.prototype.constructor = VipUser;
VipuUser.prototype.showLevel = function() {
  alert(this.level);
}

var v1 = new VipUser(‘admin’,’123456’,3);
v1.showName();
v1.showPass();
v1.showLevel();
```

### ES6 面向对象

**定义**
_ class 关键字，构造器和类分开了
_ class 里直接加方法

```javascript
class User {
  constructor(name,pass) {
      this.name = name;
      this.pass = pass;
  }

  showName() {
      alert(this.name);
  }
  showPasss() {
      alert(this.pass);
  }
}

var u1 = new User(‘admin’,’123456’);
u1.showName();
u1.showPass();

//继承
class VipUser extends User {
  constructor(name,pass,level) {
    super(name,pass);
    this.level = level;
  }
  showLevel() {
    alert(this.level);
  }
}

var v1 = new VipUser(‘admin’,’123456’,3);
v1.showName();
v1.showPass();
v1.showLevel();
```

### 对象字面量增强写法

- **属性的增强写法**
  key 和 value 一样时，保留其一

```javascript
let a = 12,
	b = 5;

let json = { a, b, c: 55 };
```

- **方法 (function)的增强写法**

```javascript
let json = {
  a: 12,
  show: function() {
    //函数体
  };
}

//以上可简写为
let json = {
  a: 12,
  show() {
    //函数体
  };
}
```

---

## JSON

### 需要转为 JSON 格式的字符串标准写法

- key 只能用双引号 `{“a”:12, ”b”:5}`
- 所有字符串 value 必须用引号包起来 `{“a”:”abc”, ”b”:5}`

### JSON 对象

1. JSON.stringify() JSON 转为字符串
2. JSON.parse() 字符串转为 JSON

---

## Promise—异步编程的解决方案

- **异步：**操作之间互不干涉，同时进行多个操作；代码更复杂；
- **同步：**同时只完成一个任务；代码简单；

### Promise 的基本使用

```javascript
new Promise((resolve, reject) => {
  //仿异步代码
	setTimeout(() => {
	  //resolve — 成功时调用
    resolve(‘success’);
    //reject — 失败时调用
    reject(‘failed’);
	}, 1000);
}).then(res => {
	//res通过resolve传入
  console.log(data);
}).catch(err => {
	//err通过reject传入
	console.log(err);
})
```

### Promise 的三种状态

在异步操作之后会有三种状态：

1. pending：等待状态，比如正在进行网络请求，或者定时器没有到时间
2. fulfill：满足状态，当主动回调了 resolve 时，就处于 fulfill 状态，并且会回调.then()
3. reject：拒绝状态，当主动回调了 reject 时，就处于 fulfill 状态，并且会回调.catch()

**另外处理形式**
在 then()里传入两个函数，第一个函数在 fulfill 状态调用，第二个函数在 reject 状态调用

```javascript
new Promise((resolve, reject) => {
	setTimeout(() => {
	  //resolve — 成功时调用
    resolve(‘success’);
    //reject — 失败时调用
    reject(‘failed’);
	}, 1000);
}).then(res => {
  console.log(data);
}, err => {
	console.log(err);
})
```

### Promise 的链式调用

有以下 Promise 嵌套，在后面两个 Promise 对象中没有进行异步操作

```javascript
new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(data1);
	}, 1000);
})
	.then(res => {
		//处理data1 -> data2
		//...
		return new Promise(resolve => {
			resolve(data2);
		});
	})
	.then(res => {
		//处理data2 -> data3
		//...
		return new Promise(resolve => {
			resolve(data3);
		});
	})
	.then(res => {
		console.log(res);
	});
```

以上代码可使用 Promise.resolve()进行简写：

```javascript
new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(data1);
	}, 1000);
})
	.then(res => {
		//处理data1 -> data2
		//...
		return Promise.resolve(data2);
	})
	.then(res => {
		//处理data2 -> data3
		//...
		return Promise.resolve(data3);
	})
	.then(res => {
		console.log(res);
	});
```

对 Promise.resolve()简写：

```javascript
new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(data1);
	}, 1000);
})
	.then(res => {
		//处理data1 -> data2
		//...
		return data2;
	})
	.then(res => {
		//处理data2 -> data3
		//...
		return data3;
	})
	.then(res => {
		console.log(res);
	});
```

### 多请求管理

```javascript
let p1 = new Promise((resolve, reject) => {
  //异步代码
	resolve(‘res1’);
	reject(‘failed’);
})
let p2 = new Promise((resolve, reject) => {
  //异步代码
	resolve(‘res2’);
	reject(‘failed’);
})

Promise.all([p1, p2]).then(data => {
	//两个请求都成功
	//data为数组：[‘res1’, ‘res2’]
  console.log(data);
}).catch(err => {
	//至少有一个请求失败
  console.log(“failed”);
})
```

### Promise + jQuery

```javascript
//谁先完成，谁先返回
//高版本jQuery自带Promise
Promise.race([
  $.ajax({...}),
  $.ajax({...})
]);
```

---

## Generator—生成器函数

- generator 函数可中断
- 以星号\*定义

```javascript
function *show() {
  alert(“a”);
  yield;	//中断
  alert(“b”);
}

//不能show()，必须定义对象
let genObj = show();
genObj.next();	//显示a
genObj.next();	//显示b
```
