---
title: nodeJs
date: 2017-10-01 11:48:12
categories: client
tags: [node,npm] 
---
# Node.js
Node.js 让 JavaScript 编写服务器端应用程序成为可能。它建立在 JavaScript V8（C++ 编写的） 运行时之上，所以它很快。最初，它旨在为应用程序提供服务器环境，但是开发人员开始利用它来创建工具，帮助他们本地的任务自动化。此后，一个全新基于 Node 工具（如 Grunt 和 Gulp）的生态系统，使得前端开发改头换面。

要使用 Node.js 中的这些工具（或包），我们需要一种有效的方式来安装和管理它们。这就要用到node 包管理器： npm 了。它能够安装你想要的包，而且提供一个强大接口来使用它们。在使用 npm 之前，首先得在系统上安装 Node.js。

## NPM（node package manager）node包管理器

>将开发者从繁琐的包管理工作（版本、依赖等）中解放出来，更加专注于功能的开发。

### package.json包描述信息

如果我们的项目依赖了很多package，一个一个地安装那将是个体力活。我们可以将项目依赖的包都在package.json这个文件里声明，然后一行命令搞定

```
npm install
```

### 安装方式

本地安装：package会被下载到当前所在目录，也只能在当前目录下使用。

全局安装：package会被下载到到特定的系统目录下，安装的package能够在所有目录下使用。'

### devDependencies和dependencies的区别
使用npm install 安装模块或插件的时候，有两种命令把他们写入到 package.json 文件里面去，比如：

--save-dev

--save

但是当安装新包的时候如何让它保持最新呢？我们可以使用 –save 标识。

在 package.json 文件里面提现出来的区别就是，使用 --save-dev 安装的 插件，被写入到 devDependencies 对象里面去，而使用 --save 安装的插件，责被写入到 dependencies 对象里面去。

那 package.json 文件里面的 devDependencies  和 dependencies 对象有什么区别呢？

devDependencies  里面的插件只用于开发环境，不用于生产环境，而 dependencies  是需要发布到生产环境的。