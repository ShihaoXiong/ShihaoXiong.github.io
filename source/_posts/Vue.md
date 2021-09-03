---
title: Vue
date: 2021-07-29 17:32:59
tags: [Web, Vue]
toc: true
categories: Notes
thumbnail: /assets/vue/cover.png
---

## 简介

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

<!-- more -->

---

## Vue.js 的安装

- CDN 引入

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="http://cdn.jsdelivr.net/nvm/vue/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="http://cdn.jsdelivr.net/nvm/vue"></script>
```

- 下载和引入
- npm 安装

---

## MVVM

![](/assets/vue/01.jpg)

## Vue 生命周期

![](/assets/vue/02.jpg)
**vue 生命周期共分为四个阶段**

1. 实例创建
2. DOM 渲染
3. 数据更新
4. 销毁实例

**共有八个基本钩子函数**

1. **beforeCreate — 创建前**
   触发的行为：vue 实例的挂载元素$el 和数据对象 data 都为 undefined，还未初始化。
   在此阶段可以做的事情：加 loading 事件
2. **created — 创建后**
   触发的行为：vue 实例的数据对象 data 有了，$el 还没有
   在此阶段可以做的事情：解决 loading，请求 ajax 数据为 mounted 渲染做准备
3. **beforeMount — 渲染前**
   触发的行为：vue 实例的$el 和 data 都初始化了，但还是虚拟的 dom 节点，具体的 data.filter 还未替换
   在此阶段可以做的事情：。。。
4. **mounted — 渲染后**
   触发的行为：vue 实例挂载完成，data.filter 成功渲染
   在此阶段可以做的事情：配合路由钩子使用
5. **beforeUpdate — 更新前**
   触发的行为：data 更新时触发
   在此阶段可以做的事情：。。。
6. **updated — 更新后**
   触发的行为：data 更新时触发
   在此阶段可以做的事情：数据更新时，做一些处理（此处也可以用 watch 进行观测）
7. **beforeDestroy — 销毁前**
   触发的行为：组件销毁时触发
   在此阶段可以做的事情：可向用户询问是否销毁
8. **destroyed — 销毁后**
   触发的行为：组件销毁时触发，vue 实例解除了事件监听以及和 dom 的绑定（无响应了），但 DOM 节点依旧存在
   在此阶段可以做的事情：组件销毁时进行提示

---

## Vue 的基本使用

```html
<div id="app">{{message}}</div>

<script>
	//创建实例
	const app = new vue({
		el: '#app', //用于挂载要管理的元素
		data: {
			//定义数据
			message: 'Hello Vue!'
		},
		method: {
			//定义方法
			m1: function () {
				//函数体
			}
		}
	});
</script>
```

---

## 模板语法

### 插值操作

#### Mustache

使用双大括号 `{{data}}` 进行插值操作；
不仅可以直接写变量，也可以写简单的表达式 ，如： `{{data1 + data2}}` ；

#### v-once

指令后面不需要跟任何表达式；
表示元素和组建只渲染一次，不会随着数据的改变而改变；

```html
<div id="app">
	<h1>{{message}}</h1>
	<h1 v-once>{{message}}</h1>
</div>

<script>
	//创建实例
	const = app new vue({
		el: '#app',
		data: {
			message: 'Hello Vue!'
		}
	})
</script>
```

#### v-html

指令后面往往跟上一个 string 类型；
会将 string 的 html 解析出来并进行渲染；

```html
<div id="app">
	<div v-html="message"></div>
</div>

<script>
	//创建实例
	const = app new vue({
		el: '#app',
		data: {
			message: '<h1>Hello Vue!</h1>'
		}
	})
</script>
```

#### v-text

作用和 Mustache 相似：用于将数据显示在界面中；
通常情况下，接受一个 string 类型；

```html
<div id="app">
	<h1 v-text="message"></h1>
</div>

<script>
	//创建实例
	const = app new vue({
	  el: '#app',
	  data: {
	    message: 'Hello Vue!'
	  }
	})
</script>
```

#### v-pre

不对 `{{ }}` 进行解析，直接显示；

```html
<div id="app">
	<h1>{{message}}</h1>
	<h1 v-pre>{{message}}</h1>
</div>

<script>
	//创建实例
	const = app new vue({
	  el: '#app',
	  data: {
	    message: 'Hello Vue!'
	  }
	})
</script>
```

#### v-cloak

在 vue 解析之前，div 中有一个属性 v-cloak；解析之后，v-cloak 删除；

### 属性绑定 (v-bind)

**简写**

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 语法糖 -->
<a :href="url"></a>
```

#### v-bind 绑定基本属性

```html
<div id="app">
  <img v-bind:src="imgUrl"></img>
</div>

<script>
  //创建实例
  const = app new vue({
    el: '#app',
    data: {
      imgUrl: 'https://...'
    }
  })
</script>
```

#### v-bind 绑定 class

```html
<style>
	.active {
		color: red;
	}
</style>

<div id="app">
	<div :class="{active:" isActive}">Hello Vue!</div>
</div>

<script>
	//创建实例
	const = app new vue({
	  el: '#app',
	  data: {
	    isActive: true
	  }
	})
</script>

<!-- 通过methods绑定 -->
<div id="app">
	<div :class="getClass()">Hello Vue!</div>
</div>

<script>
	//创建实例
	const = app new vue({
	  el: '#app',
	  data: {
	    isActive: true
	  },
	  methods: {
	    getClass: function() {
	      return {active: this.isActive};
	    }
	}
	})
</script>
```

#### v-bind 绑定 style

```html
<div id="app">
	<h1 :style="{font-size:'20px'}">{{message}}</h1>
	<h1 :style="{font-size:fontSize}">{{message}}</h1>
</div>

<script>
	//创建实例
	const = app new vue({
	  el: '#app',
	  data: {
	    message: 'Hello Vue!',
	    fontSize: 50px
	  }
	})
</script>
```

### 事件监听 (v-on)

**简写**

```html
<!-- 完整语法 -->
<button v-on:click="method"></button>
<!-- 语法糖 -->
<button @click="method"></button>
```

#### v-on 参数

如果方法不需要额外的参数，方法名后面不需要小括号 `()` 1. 在调用参数时省略了 `()` ，但是方法本身需要一个参数，这是 Vue 会默认将浏览器产生的~event~对象传入参数 2. 调用方法时，使用 `$event` 来手动获取浏览器的 event 对象

#### v-on 修饰符

1. .stop
2. .prevent
3. .{keyCode | keyAlias}
4. .native
5. .once

```html
<!-- 阻止冒泡 -->
<button @click.stop="method"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="method"></button>
<!-- 阻止默认行为，没有表达式 -->
<form @click.prevent></form>

<!-- 串联修饰符 -->
<button @click.stop.prevent="method"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter"></input>
<!-- 键修饰符，键代码 -->
<!-- enter键代码为13 -->
<input  @keyup.13="onEnter"></input>

<!-- 点击回调只会触发一次 -->
<button @click.once="method"></button>
```

### 条件判断 (v-if, v-else-if, v-else)

#### v-if 和 v-else 的基本使用

```html
<div>
	<h1 v-if="isShow">isShow为true时显示</h1>
	<h1 v-else>isShow为false时显示</h1>
</div>
```

#### v-if 和 v-else-if 的基本使用

```html
<!-- 不推荐使用v-else-if，一般用computed代替 -->
<div>
  <h1 v-if="score>=90">优秀</h1>
  <h1 v-else-if="score>=80">良好</h1>
  <h1v-else-if="score>=60">及格</h1>
  <h1 v-else>不及格</h1>
</div>
```

::为防止 vdom 重复使用组件，需要给元素添加 key 值::

### v-show

#### v-show 的基本使用

```html
<div>
  <h1 v-show="isShow">isShow为true时显示</h1>
</div
```

#### v-if 和 v-show 的区别

**v-if：**当条件为 false 时，包含 v-if 指令的元素，根本不会存在于 dom 中；
**v-show：**当条件为 false 时，v-show 只是将元素的 display 属性设置为 none；

### 循环遍历 (v-for)

#### v-for 遍历数组

```html
<div id="app">
	<ul>
		<li v-for="item" in arr">{{item}}</li>
	</ul>

	<!-- 同时获取下标 -->
	<ul>
		<li v-for="(item,index)" in arr">{{index}}. {{item}}</li>
	</ul>
</div>

<script>
	const app = new vue({
	  el: '#app',
	  data: {
	    arr: ['a1','a2','a3']
	  }
	})
</script>
```

#### v-for 遍历对象

```html
<div>
	<!-- 获取的是value -->
	<ul>
		<li v-for="item" in info">{{item}}</li>
	</ul>

	<!-- 获取的是key和value -->
	<ul>
		<li v-for="(value,key)" in info">{{key}}: {{value}}</li>
	</ul>

	<!-- 获取的是key、value、index -->
	<ul>
		<li v-for="(value,key,index)" in info">{{index}}. {{key}}: {{value}}</li>
	</ul>
</div>

<script>
	const app = new vue({
	  el: '#app',
	  data: {
	    info: {
	      name: 'Shihao',
	      age: 21,
	      height: 1.70
	    }
	  }
	})
</script>
```

#### v-for 绑定 key

```html
<!-- 绑定的key需要和item对应且唯一 -->
<ul>
	<li v-for="item" in arr" :key="item">{{item}}</li>
</ul>
```

::不建议 key 绑定 index::

#### 数组中的响应式方法

- .push() - 在最后添加一个或多个元素
- .pop() - 删除最后一个元素
- .shift() - 删除第一个元素
- .unshift() - 在最前面添加一个或多个元素
- .splice() - 删除/插入/替换某一位置的元素
  - .splice(index, howmany, ...element )
  - **删除：**第二个参数传入删除几个元素
  - **替换：**第二个元素传图替换几个元素
  - **插入：**第二个参数传入**0**，第三个参数传入要插入的元素
- .sort() - 排序
- .reverse() - 数组反转
- .$set() - vue 内部提供
  - .$set(要修改的对象,index,修改后的值)
    ::通过索引直接改变元素不是响应式::

### 表单绑定 (v-model)

v-model 实现表单元素和数据的双向绑定；

#### v-model 的基本使用

```html
<div id="app">
	<input type="text" v-model="message" />
</div>

<script>
	const app = new vue({
		el: '#app',
		data: {
			message: 'Hello Vue!'
		}
	});
</script>
```

#### v-model 原理

v-model 实质上包含两个操作： 1. v-bind 绑定一个 value 属性； 2. v-on 指令给当前元素绑定 input 事件；
`<input type="text" v-model="message" >`
等同于
`<input type="text" :value="message" @input="message=$event.target.value">`

#### v-model 结合 radio (单选框)

```html
<div id="app">
	<label for="male"> <input type="radio" v-model="sex" id="male" name="sex" value="男" />男 </label>
	<label for="female"> <input type="radio" v-model="sex" id="female" name="sex" value="女" />女 </label>
	<h2>选择的性别：{{sex}}</h2>
</div>

<script>
	const app = new vue({
		el: '#app',
		data: { sex: '' }
	});
</script>
```

#### v-model 结合 checkbox

- 单选框

```html
html
<div id="app">
	<label for="agree"> <input type="checkbox" id="agree" v-model="isAgree" />同意 </label>
	<button :disable="!isAgree">下一步</button>
</div>

<script>
	const app = new vue({
	  el: '#app',
	  data: { isAgree: false //布尔类型 }
	})
</script>
```

- 多选框

```html
html
<div id="app">
	<input type="checkbox" value="篮球" v-model="hobbies" />篮球
	<input type="checkbox" value="足球" v-model="hobbies" />足球
	<input type="checkbox" value="羽毛球" v-model="hobbies" />羽毛球
</div>

<script>
	const app = new vue({
	  el: '#app',
	  data: { hobbies: [] //数组类型 }
	})
</script>
```

#### v-model 结合 select

- 单选

```html
html
<div id="app">
	<select name="sel" v-model="fruits">
		<option value="苹果">苹果</option>
		<option value="香蕉">香蕉</option>
		<option value="">草莓</option>
	</select>
</div>

<script>
	const app = new vue({
	  el: '#app',
	  data: { fruits: '苹果' //字符串类型 }
	})
</script>
```

- 多选

```html
html
<div id="app">
	<select name="sel" v-model="fruits" multiple>
		<option value="苹果">苹果</option>
		<option value="香蕉">香蕉</option>
		<option value="草莓">草莓</option>
	</select>
</div>

<script>
	const app = new vue({
	  el: '#app',
	  data: { fruits: [] //数组类型 }
	})
</script>
```

#### 值绑定

```html
<div>
  <label v-for="item in originHobbies" :for="item">
    <input type="checkbox" :value="item" :id="item" v-model="hobbies">{{item}}</input>
  </label>
</div>

<script>
  const app = new vue({
    el: '#app',
    data: {
      hobbies: [],
      originHobbies: ['篮球', '足球', '羽毛球']
    }
  })
</script>
```

#### v-model 修饰符

- .lazy
- .number
- .trim

```html
<!-- 用户敲下回车或失去焦点时再绑定 -->
<input type="text" v-model.lazy="message" />

<!-- v-model默认绑定string类型 -->
<!-- 转换为number类型 -->
<input type="number" v-model.number="age" />

<!-- 	去掉字符串左右两边的空格 -->
<input type="text" v-model.trim="message" />
```

---

## 计算属性 (computed)

```html
<div id="app">
	<h1>{{fullName}}</h1>
	<h1>{{getFullName()}}</h1>
</div>

<script>
	//创建实例
	const = app new vue({
	  el: '#app',
	  data: {
	    firstName: 'Shihao',
	    lastName: 'Xiong'
	  },
	  computed: {
	    fullName: function() {
	      return this.firstName + ' ' + this.lastName;
	    }
	  },
	  methods: {
	    getFullName: function() {
	      return this.firstName + ' ' + this.lastName;
	    }
	  }
	})
</script>
```

### methods 和 computed

可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。
::使用 computed 性能会更好，但是如果不希望缓存，可以使用 methods 属性::

### 计算属性的 setter 和 getter

```html
<div id="app">
	<h1>{{fullName}}</h1>
</div>

<script>
	  //创建实例
	  const = app new vue({
	    el: '#app',
	    data: {
	      firstName: 'Shihao',
	      lastName: 'Xiong'
	    },
	    computed: {
	      //计算属性一般没有set方法
	//    fullName: {
	//      set: function(newValue) {},
	//      get: function() {
	//        return this.firstName + ' ' + this.lastName;
	//       }
	//    }
	      //以下为以上方式的简写
	      fullName() {
	        return this.firstName + ' ' + this.lastName;
	      }
	    }
	  })
</script>
```

---

## 过滤器 (filters)

```html
<div id="app">
	<ul>
		<li v-for="item" in price">{{item | showPrice}}</li>
	</ul>
</div>

<script>
	const app = new Vue({
	  el: '#app',
	  data: {
	    price: [85, 123, 48, 34]
	  }
	  filters: {
	    showPrice(price) {
	      //.toFixed(x) - 保留x位小数
	      return '¥' + price.toFixed(2);
	    }
	  }
	})
</script>
```

---

## 组件化开发

### 注册组件的基本步骤

1. 创建组件构造器
   - 调用 `Vue.extend()` 创建的是一个组件构造器
   - 通常在创建组件构造器时，传入 `template` 代表自定义组件的模板
   - 通常，Vue2.x 可使用[简写方式](bear://x-callback-url/open-note?id=27F8F5C4-B07C-4B76-BB03-952D04184634-1859-000004D79EA0DC61&header=%E6%B3%A8%E5%86%8C%E7%BB%84%E4%BB%B6%E7%9A%84%E7%AE%80%E5%86%99%E6%96%B9%E5%BC%8F)

```javascript
//基本创建方法
const cpnConstructor = Vue.extend({
	template: `...`
});
```

2. 注册组件
   - 调用 `Vue.component()` 将创建的组件构造器注册为一个组件，并赋予其标签名
   - 需要传递两个参数：
     1. 注册组件的标签名
     2. 组件构造器

```javascript
//全局组件
Vue.component('my-cpn', cpnConstructor);
```

3. 使用组件

```html
<my-cpn></my-cpn>
```

### 全局组件和局部组件

**全局组件：**可以在多个 Vue 实例中使用
**局部组件：**在 Vue 实例中注册

```javascript
//局部组件注册
const app = new Vue({
  el: '#app',
  data: {...},
  components: {
    'my-cpn': cpnConstructor
  }
})
```

### 父组件和子组件

```html
<div id="app">
	<cpn2></cpn2>
</div>

<script>
	//cpn1为子组件，cpn2为父组件
	const cpn1 = Vue.extend({
	  template: `...`
	})
	const cpn2 = Vue.extend({
	  template: `
	    ...
	    <cpn1></cpn1>`,
	  components: {
	    //允许在cpn2的template中使用cpn1组件
	    'cpn1': cpn1
	  }
	})

	const app = new Vue({
	  el: '#app',
	  data: {...},
	  components: {
	    'cpn2': cpn2
	  }
	})
</script>
```

### 注册组件的简写方式

省去 extend，直接使用对象代替

```javascript
//不再需要Vue.extend()
//直接注册

//全局组件
Vue.component('my-cpn', {
  template: `...`
})

//局部组件
const app = new Vue({
  el: '#app',
  data: {...},
  components: {
    'my-cpn': {
      template: `...`
    })
  }
})
```

### 组件模板抽离方法

两种定义模板的方法： 1. script 标签，type 设为 text/x-template 2. template 标签

```html
<!-- 定义模板-script标签：类型为text/x-template -->
<script type="text/x-template" id="cpn">
	//html代码
</script>

<!-- 定义模板-template标签 -->
<template id="cpn">
	<!-- html代码 -->
</template>

<script>
	//全局组件
	Vue.component('my-cpn', {
	  template: '#cpn'
	})

	//局部组件
	const app = new Vue({
	  el: '#app',
	  data: {...},
	  components: {
	    'my-cpn': {
	      template: '#cpn'
	    }
	  }
	})
</script>
```

### 组件数据的存放

- 组件对象使用 data 属性存放数据
- data 属性必须是一个**函数**
- 函数返回一个**对象**，对象内部保存数据

```html
<div id="app">
	<my-cpn></my-cpn>
</div>

<template id="myCpn">
	<div>{{message}}</div>
</template>

<script>
	const app = new Vue({
		el: '#app',
		components: {
			cpn: {
				template: '#myCpn',
				data() {
					return { message: 'Hello Vue!' };
				}
			}
		}
	});
</script>
```

### 父子组件通信

- 通过 props 向子组件传递数据
- 通过事件向父组件发送信息
  ![](/assets/vue/03.jpg)

#### 父传子 (props)

##### props 基本用法

```html
<div id="app">
	<cpn :cmovies="movies" :cmessage="message"></cpn>
</div>

<template id="cpn">
	<div>
		<p>{{cmovies}}</p>
		<h2>{{cmessage}}</h2>
	</div>
</template>

<script>
	const cpn = {
	  template: '#cpn',
	  //1. 传入数组
	  props: ['cmovies', 'cmessage'],

	  //2. 传入对象
	  props: {
		//1. 类型限制
		cmovies: Array,
		cmessage: String,

		//2. 提供一些默认值
		cmessage: {
		  type: String,
		  default: '...',
		  //设置必传属性，表示cmessage不可缺失
		  required: true
		}
		cmovies: {
		  type: Array,
		  //类型是对象或者数组时，默认值必须是一个函数
		  default() {
			return []
		  }
		}
	  }
	  data() {
		return {}
	  }
	}

	const app = new Vue({
	   el: '#app',
	  data: {
		movies: ['m1','m2','m3'],
		message: 'Hello Vue!'
	  },
	   components: {
	     cpn
	   }
	})
</script>
```

##### props 数据验证

props 可传入数组或对象，当需要对**props 进行类型验证时**，需要传入对象。
**验证支持的类型**
_ string
_ number
_ boolean
_ array
_ object
_ date
_ function
_ symbol

```javascript
Vue.component('my-cpn', {
	props: {
		//基础的类型检查 ('null'匹配任何类型)
		propA: number,

		//多个可能的类型
		propB: [String, Number],

		//必填的字符串
		propC: {
			type: String,
			required: true
		},

		//带有默认值的数字
		prosD: {
			type: Number,
			default: 100
		},

		//带有默认值的对象
		propE: {
			type: Object,
			//对象或数组默认值必须是一个工厂函数获取
			default() {
				return { message: 'hello' };
			}
		},

		//自定义验证函数
		propF: {
			validator(value) {
				//这个值必须匹配下列字符串中的一个
				return ['success', 'warning', 'danger'].indexOf(value) !== -1;
			}
		}
	}
});
```

#### 子传父 (自定义事件)

**自定义事件流程：** 1. 在子组件中，通过 `$emit()` 来触发事件 2. 在父组件中，通过 `v-on` 来监听子组件事件

```html
<div id="app">
	<cpn @itemclick="cpnClick"></cpn>
</div>

<template id="cpn">
	<div>
		<button v-for:"item in categories" @click="btnClick(item)">{{item.name}}</button>
	</div>
</template>

<script>
	//子组件
	const cpn = {
	  template: '#cpn',
	  data() {
		return {
		  categories: [
			{ id: 'aaa', name: '推荐' },
			{ id: 'bbb', name: '手机' },
			{ id: 'ccc', name: '电器' },
		  ]
		}
	  },
	  methods: {
		btnClick(item) {
		  //发射事件，自定义事件
		  this.$emit('itemclick', item);
		}
	  }
	}

	//父组件
	const app = new Vue({
	   el: '#app',
	  data: {
		movies: ['m1','m2','m3'],
		message: 'Hello Vue!'
	  },
	   components: {
	     cpn
	   },
	  methods: {
		cpnClick(item) {
		  //console.log(item);
		}
	  }
	})
</script>
```

#### 父访问子 ($children/$refs)

- 父组件访问子组件使用 `$children` 或者 `$refs`
- `this.$children` 是一个数组类型，它包含所有子组件对象
- `this.$refs` 是一个对象类型，默认是空对象

```html
<div id="app">
	<cpn></cpn>
	<cpn></cpn>
	<cpn ref="myCpn"></cpn>
	<button @click="btnClick"></button>
</div>

<template id="cpn">
	<!-- 模板代码 -->
</template>

<script>
	//子组件
	const cpn = {
		template: '#cpn'
	};

	//父组件
	const app = new Vue({
		el: '#app',
		data: {
			message: 'Hello Vue!'
		},
		components: {
			cpn
		},
		methods: {
			btnClick() {
				//$children访问子组件
				this.$children[0].data;

				//$refs访问子组件，需要在子组件上添加ref属性
				this.$refs.myCpn.data;
			}
		}
	});
</script>
```

#### 子访问父 ($parent)

```html
<div id="app">
	<cpn></cpn>
</div>

<template id="cpn">
	<div>
		<ccpn></ccpn>
	</div>
</template>

<template id="ccpn">
	<div>
		<!-- 模板代码 -->
		<button @click="btnClick"></button>
	</div>
</template>

<script>
	//子组件
	const ccpn = {
	  template: '#ccpn',
	  methods: {
		btnClick() {
		  //$parent访问父组件
		  this.$parent.data

		  //$root访问跟组件
		  this.$root
		}
	  }
	}
	const cpn = {
	  template: '#cpn',
	  data: { return { ... } },
	  components: {
		ccpn
	  }
	}

	//父组件
	const app = new Vue({
	   el: '#app',
	  data: {
		message: 'Hello Vue!'
	  },
	   components: {
	     cpn
	   }
	})
</script>
```

### 插槽 (slot)

**组件的插槽：**为了让封装的组件更加具有扩展性，让使用者决定组件内部的一些内容到底展示什么

#### slot 的基本使用

```html
<div id="app">
	<cpn>
		<!-- button会替换到slot的位置 -->
		<button>按钮</button>
	</cpn>

	<cpn>
		<!-- 多个元素会全部替换 -->
		<h1>title</h1>
		<button>按钮</button>
	</cpn>
</div>

<template id="cpn">
	<div>
		<!-- 模板代码 -->

		<!-- 插槽：预留元素位置 -->
		<slot></slot>

		<!-- 带默认的slot -->
		<slot><button>按钮</button></slot>
	</div>
</template>

<script>
	const cpn = {
	  template: '#cpn',
	}

	const app = new Vue({
	   el: '#app',
	  data: { ... },
	  components: {
		cpn
	  }
	})
</script>
```

#### 具名插槽的使用

```html
<div id="app">
	<cpn>
		<!-- 替换center插槽 -->
		<button v-slot="center"></button>
	</cpn>
</div>

<template id="cpn">
	<div>
		<!-- ...模板代码... -->
		<slot name="left"><span>左边</span></slot>
		<!-- ...模板代码... -->
		<slot name="center"><span>中间</span></slot>
		<!-- ...模板代码... -->
		<slot name="right"><span>右边</span></slot>
	</div>
</template>

<script>
	const cpn = {
	  template: '#cpn',
	}

	const app = new Vue({
	   el: '#app',
	  data: { ... },
	  components: {
		cpn
	  }
	})
</script>
```

#### 编译作用域

::父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的::

#### 作用域插槽

父组件替换插槽的标签，但是内容由子组件来提供

```html
<div id="app">
	<cpn></cpn>

	<!-- 获取子组件中的pLanguages -->
	<cpn>
		<template v-slot:default="{pLanguages}">
			<span v-for="item" in slotProps.data"></span>
		</template>
	</cpn>
</div>

<template id="cpn">
	<div>
		<slot :pLanguages="pLanguages">
			<ul>
				<li v-for="item" in pLanguages">{{item}}</li>
			</ul>
		</slot>
	</div>
</template>

<script>
	const cpn = {
	  template: '#cpn',
	  data() {
		return {
		  pLanguages: ['javascript','C++','Swift']
		}
	  }
	}

	const app = new Vue({
	   el: '#app',
	  data: { ... },
	  components: {
		cpn
	  }
	})
</script>
```

---

## 模块化开发

**模块化的两个核心：**导入和导出

### CommonJS

- CommonJS 的导出

```javascript
module.exports = {
	flag: true,
	demo(a, b) {
		return a + b;
	}
};
```

- CommonJS 的导入

```javascript
//CommonJS模块
let { flag, demo } = require('url');

//等同于
let _mA = require('url');
let flag = _mA.flag;
let demo = _mA.demo;
```

### ES6 的模块化

- html 引入
  `<script src="url" type="module"></script> `

- **export (导出)**
  export 用于导出变量/函数/类

```javascript
//info.js
let name = 'Shihao';
let age = 21;
function func() {
	//函数体
}
class Person {
	//内容
}

export { name, age, func, Person };
```

    * **export default**

```javascript
let address = "Sichuan";

//export default只允许导出一个参数
export default name

//导出函数
export default function() {
	//函数体
}
```

- **import (导入)**

```javascript
//导入参数可选
import { name, Person } from './info.js';
//统一全部导入，param为自命名参数
import * as param from './info.js';

//使用导出的类
let p = new Person();

//使用export default的导入，可自命名参数，不需要{}
import myParam from './info.js';
```

---

## Webpack

{% post_link Webpack %}

## el 和 template 的区别

**问题：** 1. 如果我们希望将 data 中的数据显示在界面中，就必须修改 index.html 2. 如果我们后面自定义了组件，也必须修改 index.html 来使用组件 3. 但是 html 模板在之后的开发中，我们并不希望手动的来频繁修改，是否可以做到？

**解决方案：**
定义 template 属性： 1. 在前面的 Vue 实例中，我们定义了 el 属性，用于 index.html 和#app 进行绑定，让 Vue 实例之后可以管理它其中 的内容 2. 这里，我们可以将 div 元素中的{{message}}内容删掉，只保留一个基本的 id 为 div 的元素但是如果我们依然希望在其中显示{{message}}的内容，我们可以再定义一个 template 属性：

```javascript
new Vue({
	el: '#app',
	template: `
	  <div id="app">{{message}}</div>
	`,
	data: { message: '...' }
});
```

::以上代码在运行时，Vue 会将 index.html 中 id 为 app 的 div 替换为 template 中的代码::

**代码简化：**
当 template 中代码过多时，为简化 Vue 实例中的代码，将 template 中的代码抽离出来创建为组件使用：

```html
<!-- App.vue -->
<!-- Vue文件，用于定义组件 -->

<template>
	<div id="app">{{message}}</div>
<template>

<script>
	export default {
	  name: "App",
	  data() {
		return {  message: '...' }
	  }
	}
</script>

<!-- 设置css样式 -->
<style scoped>

<style>
```

```javascript
import App from './app.vue';

new Vue({
	el: '#app',
	template: '<App></App>',
	components: {
		App
	}
});
```

**vue 文件封装处理：**

1. 安装 vue-loader 和 vue-template-compiler
2. 修改 `webpack.config.js` 配置

```
{
	tese:: /\.vue$/,
	use: ['vue-loader']
}
```

---

## Vue CLI

CLI (Command-Line Interface)，翻译为命令行界面，俗称脚手架。
Vue CLI 是一个官方发布的 vue.js 项目脚手架，使用 vue-cli 可以快速搭建 Vue 开发环境以及对应的 webpack 配置。

**安装：**
Vue CLI 3
`npm install -g @vue/cli`
Vue CLI 3 初始化项目
`vue create 项目名称`

拉取 2.x 模板 (旧版本)
`npm install -g @vue/cli-init`
Vue CLI 2 初始化项目
`vue init webpack 项目名称`
![](/assets/vue/04.jpg)

### runtime-compiler 和 runtime-only

**区别：**

- runtime-compiler
  template → ast → render → vdom → UI
- runtime-only
  render → vdom → UI

```javascript
const cpn = {
	template: '<div>{{message}}</div>',
	data() {
	  return { message: 'hello vue' }
	}
}

new Vue({
	el: '#app',
	render: function(createElement) {
	  //1. 普通用法：createElement('标签', {标签的属性}, [''])
	  return createElement('h2', {class: 'box'}, ['hello world'], createElement('button', ['按钮'])]);

	  //2. 传入组件对象
	  return createElement(cpn);
	}
})
```

### Vue CLI 3

**特点：**

1. vue cli 是基于 webpack 3 开发
2. 移除配置文件根目录下的 build 和 config 等目录
3. 提供了 vue ui 命令，提供了可视化配置
4. 移除了 static 文件夹，新增了 public 文件夹，并且 index.html 移动到 public 中

**目录结构：**
![](/assets/vue/05.jpg)

---

## 路由和映射

- **路由：**路由 (routing)就是通过互联的网络吧信息从源地址传输到目的地址的活动。
- **映射表：**决定了数据包的指向。

### URL 的 hash 和 HTML5 的 history

**url 的 hash：**

- url 的 hash 也就是锚点，本质上是改变 window.location 的 href 属性
- 可以直接通过 location.hash 来改变 href，到那时页面不发生刷新

**HTML5 的 history：**

- pushState：相当于入栈和出栈，可返回
- replaceState：不可返回
- go

### vue-router

vue-router 是基于路由和组件的

- 路由用于设定访问路径，将路径和组件映射起来
- 在 vue-router 的单页面应用中，页面的路径的改变就是组件的切换

**安装：**
`npm install vue-router —-save`

**配置：**

1. 导入路由对象，并调用 Vue.use(VueRouter)
2. 创建路由实例，并传入路由映射配置
3. 在 Vue 实例中挂载创建的路由实例

```javascript
//router/index.js
//导入路由
import Vue from 'vue';
import Router from 'vue-router';

import home from '../components/home.vue';
import about from '../components/about.vue';

//通过Vue.use(插件)，安装插件
Vue.use(Router);

//创建Router对象
export default new Router({
	routes: [
		{
			path: '/home',
			component: home
		},
		{
			path: '/about',
			component: about
		}
	]
});
```

**使用：**

1. 创建路由组件
2. 配置路由映射：组件和路径映射关系
3. 使用路由，通过 `<router-link>` 和 `<router-view>`

```html
<!-- App.vue -->
<template>
	<div id="app">
		<router-link to="/home">首页</router-link>
		<router-link to="/about">关于</router-link>
		<router-view></router-view>
	</div>
</template>
```

#### 路由的默认值和修改为 history

```javascript
export default new Router({
	routes: [
		{
			//设置路由默认值
			path: '',
			redirect: '/home'
		},
		{
			path: '/home',
			component: home
		},
		{
			path: '/about',
			component: about
		}
	],

	//设置为history
	mode: 'history'
});
```

#### router-link

**属性：**

- to：用于指定跳转的路径
- tag：指定 `<router-link>` 之后渲染什么组件
  `<router-link to='...' tag='button'></router-link>`
  以上渲染出 button
- replace：replace 不会留下 history 记录，指定 replace 的情况下，后退键不能返回上一个页面
- active-class：当 `<router-link>` 对应的路由匹配成功时，会自动给当前元素设置一个 router-link-active 的 class
  设置 active-class 可以修改默认名称
  _ 在进行高亮显示的导航菜单或底部 tabbar 时，会使用到该类
  _ 但是通常不会修改类的属性，会直接使用默认的 router-link-active

#### 通过代码跳转路由

```html
<!-- App.vue -->
<template>
	<div id="app">
		<button @click="'homeClick'">首页</button>
		<button @click="'aboutClick'">关于</button>
	</div>
</template>

<script>
	export default {
	  name: 'App',
	  methods: {
		homeClick() {
		  this.$router.push('/home')
		}
		aboutClick() {
		  this.$router.push('/about')
		}
	  }
	}
</script>
```

### 动态路由

```html
<!-- App.vue -->
<template>
	<div id="app">
		<router-link :to="'/user/'+ userID">用户</router-link>
		<router-view></router-view>
	</div>
</template>

<script>
	export default {
		name: 'App',
		data() {
			return {
				userID: 'shihao'
			};
		}
	};
</script>
```

```javascript
//router/index.js
export default new Router({
	routes: [
		{
			path: '/user/:userID',
			component: user
		}
	],
	mode: 'history'
});
```

```html
<!-- user.vue -->
<template>
	<div>
		<h2>用户</h2>
		<p>{{userID}}</p>
		<p>{{$route.params.userID}}</p>
	</div>
</template>

<script>
	export default {
		name: 'user',
		computed: {
			userID() {
				return this.$route.params.userID;
			}
		}
	};
</script>
```

### 路由的懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。将不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，就会更加高效。

路由的懒加载**主要作用**是将路由对应的组件打包成一个个的 js 代码块，只有在这个路由被访问到的时候才加载对应的组件。

#### 路由懒加载的效果

![](/assets/vue/06.jpg)

#### 懒加载的方式

1. 结合 Vue 的异步组件和 Webpack 代码分析

```javascript
const home = resolve => {
	require.ensure(['../components/home.vue'], () => {
		resolve(require('../components/home.vue'));
	});
};
```

2. AMD 写法

```javascript
const about = resolve => require(['../components/about.vue'], resolve);
```

3. **ES6**

```javascript
const home = () => import('../components/home.vue');
```

### 路由的嵌套

**路径和组件的关系**
![](/assets/vue/07.jpg)

**实现的步骤：**

1. 创建对应的子组件，并且在路由映射中配置对应的子路由
2. 在组件内部使用 `<router-view>` 标签

```javascript
//router/index.js
const homeNews = () => import('../components/homeNews.vue');
const homeMessage = () => import('../components/homeMessage.vue');
routes: [
	{
		path: '/home',
		component: home,
		children: [
			//默认子路由
			{
				path: '',
				redirect: 'news'
			},
			{
				//子路由path不需要"/"
				path: 'news',
				component: homeNews
			},
			{
				path: 'message',
				component: homeMessage
			}
		]
	}
];
```

```html
<!-- home.vue -->
<template>
	<div>
		<h2>首页</h2>

		<router-link to="/home/news">新闻</router-link>
		<router-link to="/home/message">消息</router-link>
		<router-view></router-view>
	</div>
</template>
```

### 参数传递

1. params 类型
   _ 配置路由格式：/router/:id
   _ 传递的方式：在 path 后面跟上对应的值 \* 传递后形成的路径：/router/123
   [动态路由](bear://x-callback-url/open-note?id=27F8F5C4-B07C-4B76-BB03-952D04184634-1859-000004D79EA0DC61&header=%E5%8A%A8%E6%80%81%E8%B7%AF%E7%94%B1)
2. query 类型
   - 配置路由格式：/router
   - 传递的方式：对象中使用 query 的 key 作为传递方式
   - 传递后形成的路径：/router?id=123

```html
<!-- App.vue -->
<template>
	<div id="app">
		<!-- <router-link to="/profile">我的</router-link> -->
		<router-link :to="{path: '/profile', query: {name: 'xsh'}}">Profile</router-link>

		<router-view></router-view>
	</div>
</template>
```

```html
<!-- profile.vue -->
<template>
	<div>
		<h2>Profile</h2>
		<p>{{$route.query.name}}</p>
	</div>
</template>
```

**通过代码传递**

```html
<!-- App.vue -->
<template>
	<div id="app">
		<button @click="'userClick'">用户</button>
		<button @click="'ProfileClick'">Profile</button>
	</div>
</template>

<script>
	export default {
	  name: 'App',
	  methods: {
		homeClick() {
		  this.$router.push('/user/' + this.userID)
		}
		profileClick() {
		  this.$router.push({
			path: '/profile',
			query: {
				name: 'xsh',
				age: 21
			  }
		  })
		}
	  }
	}
</script>
```

### $router和$route

- $router为VueRouter实例，想要导航到不同的URL，使用 `$router.push` 方法
- $route 为当前 router 跳转对象，里面可以获取 name、path、queryparams 等

### 导航守卫

监听页面的跳转

```javascript
//router/index.js
//根据不同的页面修改页面title
const routes = [
	{
		//设置路由默认值
		path: '',
		redirect: '/home'
	},
	{
		path: '/home',
		component: home,
		meta: {
			title: '首页'
		}
	},
	{
		path: '/about',
		meta: {
			title: '关于'
		},
		component: about
	}
];

const router = new Router({
	routes,
	mode: 'history'
});

//
router.beforeEach((to, from, next) => {
	//从from跳转到to
	document.title = to.matched[0].meta.title;
	next();
});

export default router;
```

::前置钩子 (beforeEach)需要主动调用 next()函数，后置钩子 (afterEach) 不需要主动调用 next()函数::
以上为全局导航守卫，除此之外还有~路由独享守卫~和~组件内守卫~

### keep-alive

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存。

`activated()` 和 `deactivated()` 两个函数只有在该组件使用了 keep-alive 时才有效

**属性**

- include — 字符串或正则表达式，只有匹配的组件会被缓存
- exclude — 字符串或正则表达式，任何匹配的组件都不会被缓存

```html
<keep-alive exclude="home,user">
	<router-view></router-view>
</keep-alive>
```

---

## Vuex

Vuex 是一个专门为 Vue.js 应用程序开发的**状态管理模式**

- 采用~集中式存储管理~应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化
- Vuex 也集成到 Vue 官方调试工具~devtools extension~，提供了诸如零配置的 time-travel 调试，状态快照导入导出等高级调试

**状态管理**：可理解为把需要多个组件共享的变量全部储存在一个对象里面，然后将这个对象放在顶层 Vue 实例中，让其他组件可以使用。

### 单页面状态管理

![](/assets/vue/08.jpg)

- State：状态，可理解为 data 中的属性
- View：视图层，可以针对 State 的变化显示不同的信息
- Actions：用户的各种操作，如点击、输入等，会导致状态等改变

### 多界面状态管理

Vuex 的基本使用

```javascript
//store/index.js
import Vue from 'vue';
import Vuex from 'vuex';

//安装插件
Vue.use(Vuex);

//创建对象
const store = new Vuex.Store({
	state: {
		//定义数据
		data: 0
	},
	//定义方法
	mutations: {},
	//执行异步代码
	actions: {},
	//类似于计算属性
	getters: {},
	//模块
	modules: {}
});

//导出store对象
export default store;
```

Views 调用 `{{$store.state.data}}`

![](/assets/vue/09.jpg)
::异步操作需要在 Actions 中完成::

### vuex-devtools 和 mutations

- vuex-devtools
  浏览器插件，跟踪 state 的改变

- mutations

```javascript
//store/index.js
const store = new Vuex.Store({
	//定义方法
	mutations: {
		func() {
			//...
		}
	}
});
```

```html
<!-- App.vue -->
<template>
	<div @click="click"></div>
</template>

<script>
	export default {
		//...
		methods: {
			click() {
				//通过commit调用mutations中的func方法
				this.$store.commit('func');
			}
		}
	};
</script>
```

### Vuex 核心概念

- State
- Getters
- Mutations
- Actions
- Modules

#### State 单一状态树

单一状态树 (Singly Source of Truth)，也称单一数据源

Vuex 推荐将所有状态信息保存在一个 store 中，以方便管理和维护

#### Getters

类似于单个组件中的~计算属性~

##### Getters 的基本使用

```javascript
//store/index.js
const store = new Vuex.Store({
	state: { counter: 5 },
	getters: {
		powerCounter(state) {
			return state.counter * state.counter;
		}
	}
});
```

```html
<!-- App.vue -->
<template>
	<div>{{$store.getters.powerCounter}}</div>
</template>
```

##### Getters 作为参数和传递参数

- 作为参数

```javascript
//store/index.js
const store = new Vuex.Store({
	state: { counter: 5 },
	getters: {
		func1(state) {
			//...
		},
		func2(state, getters) {
			//使用func1
			//getters.func1
		}
	}
});
```

- 传递参数

```javascript
//store/index.js
const store = new Vuex.Store({
	state: { counter: 5 },
	getters: {
		func1(state) {
			//...
		},
		func2(state) {
			//使用返回的函数接收View传入的参数
			return function (param) {
				return; //...
			};
		}
	}
});
```

```html
<!-- App.vue -->
<template>
	<div>{{$store.getters.func2(...)}}</div>
</template>
```

#### Mutations

Vuex 的 store 状态更新的**唯一方式：提交 Mutation**

Mutation 主要包括两部分：

1. 字符串的~事件类型 (type)~
2. 一个~回调函数 (handler)~，该回调函数的第一个参数就是 state
   [Mutations 的使用](bear://x-callback-url/open-note?id=27F8F5C4-B07C-4B76-BB03-952D04184634-1859-000004D79EA0DC61&header=vuex-devtools%E5%92%8Cmutations)

##### Mutations 传递参数

参数被称为是 mutation 的负载 (payload)

```javascript
//store/index.js
const store = new Vuex.Store({
	//定义方法
	mutations: {
		//通过第二个参数传入
		func(state, param) {
			//...
		}
	}
});
```

```html
<!-- App.vue -->
<template>
	<div @click="click(...)"></div>
</template>

<script>
	export default {
		//...
		methods: {
			click(param) {
				//通过commit调用mutations中的func方法
				this.$store.commit('func', param);
			}
		}
	};
</script>
```

##### Mutations 提交风格

除了上面通过 `commit()` 进行提交，Vue 还提供了另外一种风格，它是一个包含 type 属性的对象

```html
<script>
	export default {
		//...
		methods: {
			click(param) {
				this.$store.commit({
					type: 'funcName',
					data
				});
			}
		}
	};
</script>
```

```javascript
//store/index.js
const store = new Vuex.Store({
	mutations: {
		func(state, payload) {
			//...
			//payload为整个commit中的对象
		}
	}
});
```

##### Mutations 响应规则

Vuex 的 store 中的 state 是响应式的，当 state 中的数据发生改变时，Vue 组件会自动更新

这就要求我们必须遵守一些 Vuex 对应的规则：

1. 提前在 store 中初始化好所需要的属性
2. 当给 state 中的对象添加/删除新属性时，使用以下方法：
   1. 使用 `Vue.set(obj, 'newProp', param)` 添加属性；`Vue.delete(obj, 'prop')` 删除属性
   2. 用新对象给旧对象重新赋值

##### Mutations 类型常量

```javascript
//store/mutations-types
//定义类型常量
export const FUNC = 'func';
```

类型常量的使用：

```javascript
//store/index.js
import * from './store/mutations-types'

const store = new Vuex.Store({
	mutations: {
	  [FUNC](state, payload) {
		//...
	  }
	}
});
```

```html
<script>
	export default {
		//...
		methods: {
			click() {
				this.$store.commit(FUNC);
			}
		}
	};
</script>
```

##### Mutations 同步函数

通常情况下，Vuex 要求 Mutations 中的方法必须是同步方法，主要原因是当使用 devtools 时，可以帮助我们捕捉 Mutations 的快照。

#### Actions

Actions 类似于 Mutations，但是是用来替代 Mutations 执行异步操作

##### Actions 的基本使用

```javascript
//store/index.js
import * from './store/mutations-types';

const store = new Vuex.Store({
	mutations: {
	  funcM(state) {
		//更改state中的属性
	  }
	},
	actions: {
	  //context可理解为store对象
	  funcA(context, payload) {
		//仿异步操作更改state中的属性
		setTimeout({
		  context.commit('funcM');
		}, 1000)
	  }
	}
});
```

```html
<script>
	export default {
		//...
		methods: {
			click() {
				this.$store.dispatch('funcA', param);
			}
		}
	};
</script>
```

##### 结合 Promise

```javascript
//store/index.js
import * from './store/mutations-types'

const store = new Vuex.Store({
	actions: {
	  //context可理解为store对象
	  funcA(context, payload) {
		return new Promise((resolve) => {
		  setTimeout({
			//...
			resolve()
		    }, 1000)
		  })
	  }
	}
});
```

```html
<script>
	export default {
		//...
		methods: {
			click() {
				this.$store.dispatch('funcA', param).then(() => {
					//...
				});
			}
		}
	};
</script>
```

##### context 对象的解构

通过 `{}` 对 context 进行解构，取出需要的属性
::属性通过名字对应::

```javascript
//store/index.js
const store = new Vuex.Store({
	actions: {
		funcA({ state, commit }) {
			//...
		}
	}
});
```

#### Modules

Vue 使用单一状态树，意味着很多状态都会交给 Vuex 来管理。当应用变得非常复杂时，store 对象就有可能变得相当臃肿，为了解决这个问题，Vuex 允许我们将 store 分割成模块，而每个模块 拥有自己的 state、mutations、actions、getters 等。

```javascript
const store = new Vuex.Store({
	modules: {
		moduleA: {},
		moduleB: {}
		//...
	}
});
```

- 获取模块 state `<div>{{$store.state.moduleA}}</div>`
- Mutations 和 Getters 用法和以上一样，命名保持不一样
- 模块中 Getters 通过第三个参数访问 store 中的 state `comProp(state, payload rootState) {}`
- 模块中 Actions 的 context 可理解为可访问 store 的本模块对象

#### 项目结构

![](/assets/vue/10.jpg)
推荐将~mutations、actions、getters、modules~抽离作为模块导入 index.js，modules 每个模块推荐形成单一文件。

---

## 网络模块封装 (axios)

### 功能特点

- 在浏览器中发送 XMLHttpRequests 请求
- 在 node.js 中发送 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求和响应数据
- ...

### axios 请求方式

支持多种请求方式：

- axios(config)
- axios.request(config)
- axios.get(url[, config])
- axios.delete(url[, config])
- axios.head(url[, config])
- axios.post(url[, data[, config]])
- axios.put(url[, data[, config]])
- axios.patch(url[, data[, config]])

### axios 的基本使用

**安装**
`npm install axios —-save`

**使用**

```javascript
import axios from 'axios';

//支持Promise，内置resolve，可直接调用.then()
axios({
	//httpbin.org可用于网络模拟
	//123.207.32.32:8000/home/multidata可用于相关测试
	//123.207.32.32:8000/home/data?...可用于相关测试 (含参数)
	url: 'httpbin.org',
	method: 'get',
	//针对get请求的参数拼接
	params: {}
}).then(res => {
	//...
});
```

### 发送并发请求

- 使用 `axios.all([])` 发送并发请求
- `axios.all([])` 返回的结果是一个数组，使用 `axios.spread` 可讲数组[res1, res2]展开为 res1，res2

```javascript
axios.all([axios({...}),axios({...})]).then(axios.spread((res1, res2) => {...}))
```

### 全局配置

BaseURL
`axios.defaults.baseURL = '...';`
`axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';`

**常见的配置选项**

- 请求地址：url: '/user'
- 请求类型：method: 'get'
- 请根路径：baseURL: 'http://...'
- 请求前的数据处理：transformRequest: [function(data) {}]
- 请求后的数据处理：transformResponse: [function(data) {}]
- 自定义的请求头：headers: {'x-Requested-With':'XMLHttpRequest'}
- URL 查询对象：params: {id: 12}
- 查询对象序列化函数：paramsSerializer: function(params) {}
- request body：data: {key: value}
- 超时设置：timeout: 1000
- 跨域是否带 Token：withCredentials: false
- 自定义请求处理：adapter: function(resolve, reject, config) {}
- 身份验证信息：auth: {uname: '', pwd: '123'}
- 响应的数据格式 (json/blob/document/arraybuffer/text/stream)：responseType: 'json'

### axios 实例和模块封装

#### 实例创建

```javascript
const instance1 = axios.create({
	baseURL: 'http://ip1'
	timeout: 1000
})

instance1({
	url: '...',
	//...
}).then(res => {
	//...
})

const instance2 = axios.create({
	baseURL: 'http://ip2'
	timeout: 1000
})

instance2({
	url: '...',
	//...
}).then(res => {
	//...
})
```

#### 模块封装

##### 基本封装

```javascript
//network/request.js
import axios from 'axios';

export function request(config, success, failure) {
	//创建axios实例
	const instance = axios.create({
		baseURL: 'http://...',
		timeout: 1000
	});

	//发送真正的网络请求
	instance(config)
		.then(res => {
			success(res);
		})
		.catch(err => {
			failure(err);
		});
}
```

导入调用

```javascript
import { request } from './network/request.js';

request(
	{
		url: '...'
	},
	res => {},
	err => {}
);
```

##### 方案 2

```javascript
//network/request.js
import axios from 'axios';

export function request(config) {
	return new Promise((resolve, reject) => {
		const instance = axios.create({
			baseURL: 'http://...',
			timeout: 1000
		});
	});

	instance(config)
		.then(res => {
			resolve(res);
		})
		.catch(err => {
			reject(err);
		});
}
```

导入调用

```javascript
import {request} from './network/request.js';

request({
	url: '...'
}).then(res => {...}).catch(err => {...})
```

##### 最终方案

```javascript
//network/request.js
import axios from 'axios';

export function request(config) {
	const instance = axios.create({
		baseURL: 'http://...',
		timeout: 1000
	});

	return instance(config);
}
```

导入调用

```javascript
import {request} from './network/request.js';

request({
	url: '...'
}).then(res => {...}).catch(err => {...})
```

### axios 拦截器

axios 提供了拦截器，用于在发送每次请求或得到响应后，进行对应的拦截

```javascript
//全局拦截
axios.interceptors()

//实例拦截
//请求拦截
instance.interceptors.request.use(config => {
	//请求拦截的作用
	//比如config中的一些信息不符合服务器的要求

	//比如每次发送网络请求时，都希望在界面中显示一个请求图标

	//某些网络请求(比如登陆token)必须携带某些信息
	return config;
}, err => {...})

//响应拦截
instance.interceptor.response.use(res => {
	//拦截后操作...
	//res.data为需要的数据
	return res.data;
}, err => {
	console.log(err);
})
```
