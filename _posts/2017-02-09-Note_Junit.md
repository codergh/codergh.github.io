---
layout: post
title: 'Junit 小技巧总结'
tags:
  - 技巧总结
  - 日记
category: Java
---
Junit是一种很重要的手段来保证下一次的修改没有出现BUG，所以在写Junit的时候会遇到很多奇奇怪怪的问题。下面就是总结一些经常遇到的问题， 然后针对这些问题提出一些解决方案。
<!--more-->
# 静态类mock
有一个静态类，静态类里面有一个静态的autowired的属性， 在被测试类里面用到了这个静态类， 怎么来进行Mock？

    StaticCalss staticClassMock = mock(StaticCalss.class);
    ReflectionTestUtils.setField(StaticCalss.class, "propertyName", staticClassMock);

# 待续
