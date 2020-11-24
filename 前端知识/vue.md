## vue的介绍

Vue 是一个渐进式的框架，只注重视图层，结合了HTML+CSS+JS,非常易用，生态好，体积小，速度快。



## MVVM开发模式

M model 数据

V  view 试图

VM viewModel 连接视图和数据中间件

model 和 view 只能通过 viewModel 通信

viewModel 可以观察数据变化，并对视图对应的内容进行更新

viewModel 能具体视图变化，并能通知数据改变



## 快速上手

#### 引入cdn vue.js

<script src="https://cdn.bootcdn.net/ajax/libs/vue/3.0.2/vue.cjs.js"></script>

#### 绑定元素和实例化vue

```html
#html
<div  id="app">
	{{ name }}
</div>

#js
new Vue({
	el:"app",
	data:{
		name:"tjx"
	}
})

```



#### 插值表达式

插值表达式是在html中被绑定元素中的，目的是通过插值表达式来获取vue 对象中的属性和方法



