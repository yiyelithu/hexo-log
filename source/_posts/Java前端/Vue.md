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

![lifecyc](/images/client/vue lifecycle.png)

生命周期钩子

![life](/images/client/vue Lifecycle hooks.png)

### 双向绑定

v-model 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

### 数据
 
如果你知道你会在晚些时候需要一个属性，但是一开始它为空或不存在，那么你仅需要设置一些初始值。比如：

```aidl
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```