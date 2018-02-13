---
title: npm scripts
date: 2018-01-14 11:48:12
categories: client
tags: [node,npm] 
---
### 一、什么是 npm 脚本
npm 允许在package.json文件里面，使用scripts字段定义脚本命令。
```sql
{
  // ...
  "scripts": {
    "build": "node build.js"
  }
}
```
上面代码是package.json文件的一个片段，里面的scripts字段是一个对象。它的每一个属性，对应一段脚本。比如，build命令对应的脚本是node build.js。
命令行下使用npm run命令，就可以执行这段脚本。
```sql
$ npm run build
# 等同于执行
$ node build.js
```
这些定义在package.json里面的脚本，就称为 npm 脚本。它的优点很多
1. 项目的相关脚本，可以集中在一个地方。
2. 不同项目的脚本命令，只要功能相同，就可以有同样的对外接口。用户不需要知道怎么测试你的项目，只要运行npm run test即可。
3. 可以利用 npm 提供的很多辅助功能。

查看当前项目的所有 npm 脚本命令，可以使用不带任何参数的npm run命令。
```sql
$ npm run
```
### 二：执行顺序
如果 npm 脚本里面需要执行多个任务，那么需要明确它们的执行顺序。
如果是并行执行（即同时的平行执行），可以使用&符号。
```sql
$ npm run script1.js & npm run script2.js
```

如果是继发执行（即只有前一个任务成功，才执行下一个任务），可以使用&&符号
```sql
$ npm run script1.js && npm run script2.js
```

### 应用
在 `package.json` 添加以下代码执行`npm run gg` 可以连续执行（hexo g）（hexo d）俩个命令，这样就不用每次执行俩个命令
```sql
 "scripts": {
    "gg": "hexo g && hexo d"
  }
```
详见:http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html