---
title: mmspa2 released
tags:
  - mmspa
  - multimodal route planning
  - route planning
  - routing
id: 144
categories:
  - Programming
date: 2016-01-12 21:13:36
---

这个工作缘于我在指导实验室今年的一篇硕士论文时蹦出来的一个想法。本来，我是想在这篇论文中把[CH](https://www.wikiwand.com/en/Contraction_hierarchies)应用到mmrp中，通过减少IO来提高mmspa的整体性能。但后来我发现我想错了，因为我错误的理解了CH。CH并不是把道路网进行类似影像金字塔那样的多层次组织，尽管它的名字叫“分层压缩”，而是通过在原始网络中增加很多的“快捷边”，让后续的路径搜索算法可以“绕过”很多不必要去考察的顶点，从而大幅减少松弛操作的次数，达到加速最短路径搜索的目的。当然了，这种加速是有代价的，那就是在事先寻找、确定并建立这些“捷径边”所需的时间开销，以及增加了这些捷径边所带来的空间开销。

虽然应用CH来改善mmspa的IO看来暂时无望，但我在去年年底又想到了另一个可以改善目前每次计算都慢得要命的办法，尽管有点简单粗暴。那就是在程序启动时就把所有mode的图都读进来，然后后面针对每次routing plan进行graph装配的时候都不要再去访问数据库了（那个读graph的sql着实费时），而是从内存里的这个graph cache中取数据，装配成临时的graph集合，然后计算结束后再释放掉这些临时图。

顺着这个思路，我就开始着手于去年12月中旬实现这个feature，顺便也把mmspa这个老库好好整理了一下，动了动手术。最终在1月5号发布了2.0版本的mmspa，在增加了内存开销的代价下，把单次路径规划的性能提升了大约10倍，但到了http服务的层次，这个提升被打折到了5倍左右。

这次改造，对代码的结构进行了大幅调整，但仍然保留了原来的主要实现方式，没有动基础的算法和数据结构。尽管如此，过程也并不是一帆风顺。最主要的障碍就是装配公交系统graph，因为按照新方法组装起来的graph中的vertex不再是按照id排序的。所以只能又加入了一个快排函数，专门为公交graph进行顶点排序。这里费了一点时间。最后，用valgrind彻底清理了一遍所有内存泄漏的问题，确实找到了好几个，而且其中还有当年留下的千年老bug。昨天，我又发布了一个[2.1.0](https://github.com/tumluliu/mmspa/releases/tag/v2.1)版本，删除了对原来1.x版本的接口兼容（因为我知道受影响的只有我自己的pymmrouting），但也保留了能兼容1.x版本接口的[2.0.1](https://github.com/tumluliu/mmspa/releases/tag/v2.0.1)版本。

至此，虽然还有很多可以改进的东西（都写在[issues](https://github.com/tumluliu/mmspa/issues)里面了），但这次的工作暂时告一段落。路径规划，multimodal路径规划，是个永无止境的工作，而且就如杨剑说的，后面的优化工作量会非常大，而带来的收益会越来越小。这也算是一种边际效应递减吧。不知道今后是否还有机会继续[3.0](https://github.com/tumluliu/mmspa/milestones/v3.0)的工作，抑或把它变成未来某块工作的基础，那也算是幸事一件。