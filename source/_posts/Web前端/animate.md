---
title: 　animate.css
date: 2018-03-03 10:52:12
categories: client
tags: [css] 
---
### 一 前言:
#### 背景： 
在看其他人的项目的时候发现其动画效果做的不错，通过看人家的代码发现用了这个`animate.css`这个css组件，使用起来也是特别的方便，所以就把他copy到项目中来了，顿时档次就上升了

#### 简介:
`animate.css` 是一个来自国外的 CSS3 动画库，它预设了抖动（shake）、闪烁（flash）、弹跳（bounce）、翻转（flip）、旋转（rotateIn/rotateOut）、淡入淡出（fadeIn/fadeOut）等多达 60 多种动画效果，几乎包含了所有常见的动画效果。而且使用起来也是特别方便

官网传送门: https://daneden.github.io/animate.css/

在官网上有示例动画，主页也十分简洁，同时也提供了代码下载, 也可以看看这篇博客写的例子 `https://www.cnblogs.com/xiaohuochai/p/7372665.html`

### 二 如何使用：

#### 步骤：
1. 在官网上下载 `animate.css` , 把他导入到项目中来, 也可以使用cdn `https://unpkg.com/animate.css@3.5.2/animate.min.css`
2. 代码示例
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Document</title>
        <link rel="stylesheet" href="https://unpkg.com/animate.css@3.5.2/animate.min.css">
        <style>
            .box{height: 100px;width: 100px;background-color: lightblue}
        </style>
    </head>
    <body>
        <div class="box animated flash"></div>
    </body>
</html>
```
3. 只要在元素中class 添加 `animated` 和相应的动画class名就可以实现动画效果, 当然也可以通过js动态设置class





