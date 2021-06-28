---
title: react
date: 2021-06-28 17:40:01
tags: [Web, React]
toc: true
cover: /assets/react/cover.png
thumbnail: /assets/react/cover.png
---

## 骨架

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<title>React</title>
	</head>
	<body>
		<!-- 准备一个容器 -->
		<div id="test"></div>
	</body>

	<!-- 引入React核心库 -->
	<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
	<!-- 引入React-dom，用于支持react操作dom -->
	<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转换为js -->
	<script src="./babel.min.js"></script>

	<script type="text/babel">
		// 创建虚拟DOM
		const VDOM = <div>React</div>;
		// 渲染虚拟DOM到页面
		ReactDOM.render(VDOM, document.getElementById('test'));
	</script>
</html>
```

---

## jsx 语法规则

1. 定义虚拟 DOM 时，不要写引号
2. 标签中混入 JS 表达式要用 `{}`
3. 样式的类名指定不要用 `class` ，要使用 `className`
4. 内联样式要用 `style={{key: value}}` 的形式去写
5. 只有一个根标签
6. 标签必须闭合
7. 标签首字母
   - 若小写字母开头，则将标签转为 html 中同名元素；若 html 中无该标签对应的同名元素，则报错
   - 若大写字母开头， react 就去渲染对应的组件，若组件未定义，则报错

```javascript
// 创建虚拟DOM
const VDOM = (
	<div className='react-container'>
		<span style={{ color: 'blue' }}>React</span>
	</div>
);
// 渲染虚拟DOM到页面
ReactDOM.render(VDOM, document.getElementById('test'));
```

**示例**
::注意区分 js 表达式和和 js 语句::

1. 表达式：一个表达式会产生一个值，可以放在任何一个需要的地方
   - a
   - a + b
   - demo(1) ~有返回值~
   - arr.map()
   - function test () {}
2. 语句（代码）

```javascript
const data = ['Angular', 'React', 'Vue'];

// 创建虚拟DOM
const VDOM = (
	<div className='react-container'>
		<h1>列表渲染</h1>
		<ul>
			{data.map(item => {
				return <li key={item}>{item}</li>;
			})}
		</ul>
	</div>
);
// 渲染虚拟DOM到页面
ReactDOM.render(VDOM, document.getElementById('test'));
```

---

## 模块与组件、模块化与组件化

- 模块：向外提供特定功能的 js 程序，一般就是一个 js 文件
- 组件：用来实现局部功能效果的代码和资源的集合（html/css/js/image...）
- 模块化：当应用的 js 都以模块来编写，该应用就是一个模块化应用
- 组件化：当应用是以多组件的方式实现，该应用就是一个组件化应用

---

## 组件

### 函数式组件

```javascript
// 创建函数式组件
function Component() {
	console.log(this); // undifined babel编译后开启了严格模式
	return <h2>函数定义的组件，适用于简单组件的定义</h2>;
}

// 渲染组件到页面
ReactDOM.render(<Component />, document.getElementById('test'));
```

### 类式组件

1. 必须继承 React.Component
2. 必须包含 `render()` 函数
3. `render()` 必须有返回值

```javascript
// 创建类式组件
// 必须继承React.Component
class Component extends React.Component {
	render() {
		console.log(this); // Component的（组件）实例对象
		return <h1>类定义的组件，适用于复杂组件的定义</h1>;
	}
}

// 渲染组件到页面
ReactDOM.render(<Component />, document.getElementById('test'));
```

---

## 组件实例三大核心属性

### State

```javascript
class Component extends React.Component {
	// 构造器中是否接收props，是否传递给super，取决于是否希望在构造器中通过this访问props
	constructor(props) {
		// 必须调用super()
		super(props);
		// state必须是一个对象
		this.state = { status: true };
	}

	render() {
		return <h1>State: {this.state.status ? 'state1' : 'state2'}</h1>;
	}
}

ReactDOM.render(<Component />, document.getElementById('test'));
```

#### setState

状态不能直接更改，需要通过 `setState` 进行更新，更新是合并操作，不是替换

```javascript
class Component extends React.Component {
	constructor(props) {
		// 必须调用super()
		super(props);
		// state必须是一个对象
		this.state = { status: true, status2: true };
		// 解决switchStatus中this指向问题
		this.switchStatus = this.switchStatus.bind(this);
	}

	render() {
		// switchStatus不能添加小括号
		return <h1 onClick={this.switchStatus}>State: {this.state.status ? 'state1' : 'state2'}</h1>;
	}

	switchStatus() {
		let status = this.state.status;
		// 状态里的值不能直接更改
		this.setState({ status: !status });
	}
}

ReactDOM.render(<Component />, document.getElementById('test'));
```

::构造器执行了 1 次，render()函数执行了 1 + n 次::

#### state 的简写方式

```javascript
class Component extends React.Component {
	state = { status: true, status2: true };

	render() {
		return <h1 onClick={this.switchStatus}>State: {this.state.status ? 'state1' : 'state2'}</h1>;
	}

	// 箭头函数没有this指向，若在箭头函数中使用了this，箭头函数会找外层的this作为内部this的指向
	switchStatus = () => {
		let status = this.state.status;
		this.setState({ status: !status });
	};
}

ReactDOM.render(<Component />, document.getElementById('test'));
```

#### State 总结

1. 组件中 `render` 方法中的 this 为组件实例对象
2. 组件自定义的方法中 this 为 undefined，如何解决？
   - 强制绑定 this：通过函数对象的 `bind()`
   - 箭头函数
3. 状态数据，不能直接修改或更新，需要通过 `setState`

### Props

::Props 是只读的，不允许直接修改::

#### 基本使用

```javascript
class Component extends React.Component {
	render() {
		console.log(this);
		return (
			<div>
				<h2>参数：{this.props.param}</h2>
			</div>
		);
	}
}

ReactDOM.render(<Component param='param' />, document.getElementById('test'));
```

#### 批量传递 props

```javascript
class Component extends React.Component {
	render() {
		const { param1, param2 } = this.props;
		return (
			<div>
				<h2>参数1：{param1}</h2>
				<h2>参数2：{param2}</h2>
			</div>
		);
	}
}

const params = { param1: 'param1', param2: 'param2' };
ReactDOM.render(<Component {...params} />, document.getElementById('test'));
```

#### 对 props 进行限制

在 16 版本以前可通过 `React.PropTypes. ...` 进行类型限制
16 版本以后需要引入 `PropTypes`

```javascript
class Component extends React.Component {
	render() {
		const { param1, param2 } = this.props;
		return (
			<div>
				<h2>类型限制：{param1}</h2>
				<h2>值限制：{param2}</h2>
			</div>
		);
	}
}

// 16版本以前写法
// Component.propsTypes = {
// 	param1: React.PropsTypes.string
// };

// 16版本以后写法
Component.propTypes = {
	// 限制类型
	param1: PropTypes.string,
	// 限制类型为函数
	paramFunc: PropTypes.func,
	// 必传
	param2: PropTypes.number.isRequired
};
// 默认值
Component.defaultProps = {
	param1: '参数1'
};

ReactDOM.render(<Component param1='param1' param2={2} />, document.getElementById('test'));
```

#### Props 的简写

通过 `static` 将属性直接加在组件类上

```javascript
class Component extends React.Component {
	static propTypes = {
		param1: PropTypes.string,
		paramFunc: PropTypes.func,
		param2: PropTypes.number.isRequired
	};
	static defaultProps = { param1: '参数1' };

	render() {
		const { param1, param2 } = this.props;
		return (
			<div>
				<h2>类型限制：{param1}</h2>
				<h2>值限制：{param2}</h2>
			</div>
		);
	}
}
```

#### 函数式组件使用 Props

```javascript
function Component(props) {
	return (
		<div>
			<h2>{props.params}</h2>
		</div>
	);
}

Component.propTypes = { params: PropTypes.string.isRequired };
Component.defaultProps = { params: 'defaultParams' };

// 渲染组件到页面
ReactDOM.render(<Component />, document.getElementById('test'));
```

---

## 事件绑定

```javascript
class Component extends React.Component {
	constructor(props) {
		// 必须调用super()
		super(props);
		// state必须是一个对象
		this.state = { status: true };
		// 解决switchStatus中this指向问题
		this.switchStatus = this.switchStatus.bind(this);
	}

	render() {
		// switchStatus不能添加小括号
		return <h1 onClick={this.switchStatus}>State: {this.state.status ? 'state1' : 'state2'}</h1>;
	}

	switchStatus() {
		// switchStatus作为onClick回调，不是通过实例调用，而是直接调用，
		// 类中的方法默认开启了局部的严格模式，所以this为undefined
		// 需要使用 this.switchStatus = this.switchStatus.bind(this);改变this指向
		console.log(this); // undefined
		this.state.status = !this.state.status;
	}
}

ReactDOM.render(<Component />, document.getElementById('test'));
```

::通过箭头函数实现事件绑定 见 state 的简写方式::
