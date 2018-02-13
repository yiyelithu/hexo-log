---
title: v-model v-bind
date: 2017-10-05 10:12:12
categories: client
tags: [vue] 
---
Mustache (双大括号写法)不能在 HTML 属性中使用，应使用 v-bind 指令：
```aidl
<div v-bind:id="dynamicId"></div>
```

这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：
```aidl
<button v-bind:disabled="someDynamicCondition">Button</button>
```
1. v-bind动态绑定指令，默认情况下标签自带属性的值是固定的，在为了能够动态的给这些属性添加值，可以使用v-bind:你要动态变化的值="表达式"

2. v-bind用于绑定属性和数据 ，其缩写为“ : ” 也就是v-bind:id  === :id  

3. v-model用在表单控件上的，用于实现双向数据绑定，所以如果你用在除了表单控件以外的标签是没有任何效果的。

> v-bind
```aidl
<!-- 绑定一个属性 -->
<img v-bind:src="imageSrc">
<!-- 缩写 -->
<img :src="imageSrc">
<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName">
<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>
<!-- 绑定一个有属性的对象 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>
<!-- 通过 prop 修饰符绑定 DOM 属性 -->
<div v-bind:text-content.prop="text"></div>
<!-- prop 绑定。“prop”必须在 my-component 中声明。-->
<my-component :prop="someThing"></my-component>
<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
<child-component v-bind="$props"></child-component>
<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```