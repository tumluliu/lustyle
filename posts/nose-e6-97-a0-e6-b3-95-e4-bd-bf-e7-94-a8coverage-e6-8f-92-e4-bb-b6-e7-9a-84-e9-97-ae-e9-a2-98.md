---
title: nose无法使用coverage插件的问题
tags:
  - coverage
  - nose
  - Programming
  - Python
  - 测试
  - 覆盖率
id: 89
categories:
  - Programming
date: 2015-06-14 10:18:01
---

OS X系统自带的Python会预装nose，但这个nose的coverage插件是用不了的，尽管可以通过nosetests &#8211;plugins看到Plugin coverage在列表中。万能的Google加Stackoverflow只找到一个问题，是通过pip install coverage解决的，但居然在我这里无效，而且如果pip install nose也不会有任何反应，因为pip会认为Requirement already satisfied。那么最后怎么解决呢？只能pip install &#8211;upgrade nose，把它升到最新的1.3.7，然后就ok了。大胆猜测，应该通过pip uninstall再重新install一遍也可以解决。

随手记的东西，就不整理格式了。