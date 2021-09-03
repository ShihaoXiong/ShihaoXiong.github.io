---
title: Ajax
date: 2021-07-09 15:56:31
tags: [Web, JavaScript, Ajax]
toc: true
categories: Notes
---

[Ajax 教程](https://www.w3school.com.cn/ajax/index.asp)

<!-- more -->

---

## Ajax 的使用

1. 创建 XMLHttpRequest 对象；

```javascript
//兼容处理
var xhr = null;
if (window.XMLHttpRequest) {
	xhr = new XMLHttpRequest();
} else {
	//IE6及以下版本
	xhr = new ActiveXObject('Microsoft.XMLHTTP');
}
```

2. 准备发送网络请求；

```javascript
//get请求
xhr.open('get', 'url?key=' + value, true);

//post请求
xhr.open('post', 'url', true);
```

3. 开始发送网络请求；

```javascript
//get请求
xhr.send(null);

//post请求
var param = 'key=' + value;
//设置xhr端请求头信息，仅适用于post请求
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
xhr.send(param);
```

4. 制定回调函数；

```javascript
xhr.onreadystatechange = function () {
	if (xhr.readyState == 4) {
		if (xhr.status == 200) {
			//返回除XML之外的格式
			var result = xhr.responseText;
			//返回XML
			var result = xhr.responseXML;
		}
	}
};
```

**readyState**

- readyState=0：xhr 对象创建完成；
- readyState=1：已经发送了请求；
- readyState=2：浏览器已经收到了服务器响应的数据；
- readyState=3：正在解析数据；
- readyState=4：数据已经解析完成，可以使用；

**status**

- 200：响应成功；
- 404：没有找到请求的资源；
- 500：服务器端错误；

---

## 同步与异步

js 中异步原理：单线程+事件队列

---

## 数据格式

**数据格式：**将数据通过一定的规范组织起来

### xml 数据格式

xml 数据格式是将数据以**标签**的方式进行组装 ，必须以 `<?xml version="1.0" encoding="USF-8" ?>` 开头。
**缺点：**体积太大，传输慢，原数据太多，解析不方便；

### Json 数据格式

**优点：**体积小，传输快，解析方便；
**Json 格式字符串转换为对象：** `JSON.parse()`

---

## jQuery 中的 Ajax

- $.ajax({})
- $.get(url(+param),function(){})
- $.post(url,{key:value},function(){})

---

## 跨域

**同源策略：** 1. 协议相同 (http/https)； 2. 域名相同； 3. 端口号相同；

### 引入外部 js 文件

`<script type="text/javascript" src="http://..."></script>`

### 动态创建 script 标签 (src)

```javascript
//DOM操作
var script = document.createElement('script');
script.src = 'http://...?key=' + value;
var head = document.querySelector('head');
head.appendChild(script);
```

### 动态制定回调函数

```javascript
window['函数名'] = function () {
	//函数体
};
```

---

## jQuery Ajax 跨域

```javascript
$.ajax({
  url: ...,
  ...,
  //设置dataType的值为jsonp来跨域获取
  dataType: "jsonp",
  //设置jsonp的值来修改回调函数的key值(callback/cb)，默认callback
  //设置jsonpCallpack的值来修改回调函数的value(函数名)
  jsonp: "cb",
  jsonpCallback: "fun"
  //成功后
  success: function(data) {
    //data为返回的数据
  }
})
```

---

## 模版引擎 (artTemplate)

### 使用步骤

1. 引入 js 文件；
2. 定义模板；

```html
<script type="text/html" id="resultTemplate">
	//循环生成 {{each arr as value i}} //模板html代码, 数据绑定使用`{{}}` {{/each}} //条件判断 {{if isShow}}
	//模板html代码 {{/if}}
</script>
```

3. 将数据和模板结合起来生成 html 片段；

```javascript
//template方法的含义就是将数据和模板结合起来，生成html片段
//第一个参数为模板id，第二个参数为传入的数据
var data = {
  arr: [...],
  isShow: true
}
var html = template("resultTemplate",data);
```

4. 将 html 片段渲染到界面中；

### 基本语法

- 得到数据中的值 `{{value}}`
- 循环操作 `{{each result as value i}} {{/each}}`
- 转义 `{{#value}}`
- 条件判断 `{{if ...}} {{/if}}`
  ::有时可能需要对原始数据进行加工操作::
