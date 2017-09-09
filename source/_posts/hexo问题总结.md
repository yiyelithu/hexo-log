---
title: Hexo问题总结
date: 2017-09-09 15:07:12
categories: 技术
tags: [hexo,github] 
---
# Hexo问题总结


- **hexo部署后，CNAME会被自动删除，怎么办？**

&nbsp;&nbsp;&nbsp;&nbsp;准确来说 CNAME 文件是放在 hexo 项目下的 source 目录，你再运行下hexo generade
然后你再去 public 目录中看看就明白了BTW，为了达到更有说服力的验证，最好在开始前先运行下hexo clean
这样会先删除 public 目录

- **HEXO发布到Github上，README.md文件正常显示的解决**

&nbsp;&nbsp;&nbsp;&nbsp;使用hexo d 发布本地编译过的代码到github上的时候，发现这个README.md文件也被解析的乱七八糟的，不是一般的github项目里面的README.md文件的显示样式，查了下，在最外层的_config.yml里面把
skip_render: README.md
添加这个配置，就OK啦。


- **指定端口启动**

    ` hexo server -p 4001`

- **如何在markdowm中添加本地图片**
	
    建议将图片统一放在 `source/images` 文件夹中。然后通过绝对路径` ![](/images/image.jpg)` 引用


