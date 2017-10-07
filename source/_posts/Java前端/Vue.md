---
title: Vue.js
date: 2017-10-03 22:40:12
categories: client
tags: [vue] 
---
### 前缀 $，实例属性与方法
这些只是Vue的命名规则，为了缺分普通变量属性，避免我们自己声明或者添加自定义属性导致覆
### 生命周期
beforecreated：el 和 data 并未初始化 

created:完成了 data 数据的初始化，el没有

beforeMount：完成了 el 和 data 初始化 

mounted ：完成挂载

<!--more-->

生命周期

![lifecyc](/images/client/lifecycle.jpg)

生命周期钩子

![life](/images/client/vue Lifecycle hooks.png)

钩子函数

钩子函数就是指再所有函数执行前，我先执行了的函数，即 钩住 我感兴趣的函数，只要它执行，我就先执行,这个解释666

### 双向绑定

v-model 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，在应用状态改变时，Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。

### 数据
 
如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值。比如：

```json
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```
### 一个对象的 v-for

```html
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```
```angularjs
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```
索引 key value

````html
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
````

```html
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身 (比如不是子元素) 触发时触发回调 -->
<div v-on:click.self="doThat">...</div>
```





