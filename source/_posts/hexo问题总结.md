---
title: Hexo问题总结
date: 2017-09-09 15:07:12
categories: 技术
tags: [hexo,github] 
---
# Hexo问题总结
1. **hexo部署后，CNAME会被自动删除，怎么办？**

准确来说 CNAME 文件是放在 hexo 项目下的 source 目录，你再运行下hexo generade
然后你再去 public 目录中看看就明白了BTW，为了达到更有说服力的验证，最好在开始前先运行下hexo clean
这样会先删除 public 目录




