---
title: Multimodal route planner demo for Munich is online
tags:
  - multimodal routing
  - path finding
  - Programming
  - route planning
id: 116
categories:
  - Programming
date: 2015-07-04 23:21:17
---

尽管还是算的很慢（平均一次请求的处理时间要10s左右），而且bug也不少，但这个三年前就应该上线的[多模式路径规划演示系统（Multimodal route planner demo for Munich）](http://luliu.me/projects/mmrp)还是可以用了，源码还是在[github](https://github.com/tumluliu/mmrp-web)上。除了核心算法库（[mmspa](https://github.com/tumluliu/mmspa)）几乎没变以外，其它部分全都重写了。前端部分的代码大量参考了Mapbox的mapbox.directions.js项目，但我并没有fork它，而是基于它的代码进行了修改，这在它的license中是允许的。另外，从底图到样式，也都利用了Mapbox提供的多个服务和开源项目，所以这里这里要特别对Mapbox的开发人员们致以诚挚的谢意。

我还会继续不断改善它，改善它的性能、规划质量、稳定性、交互体验等等，要让它变得越来越好用，因为，它主要是**给我自己用的**。后面用于验证新想法的实验都会基于这个演示平台来做，希望能有些收获。