---
layout: post
title: 'Spring 总结'
tags:
  - 技巧总结
  - 日记
category: spring
---
学而时习之则不亦悦乎。
<!--more-->
# MVC中controller的线程安全问题

* Srping MVC中被@Controller注解的类都是单例的， 若是某个controller中有一个私有变量i， 所有请求到同一个controller时使用的i是共用的， 即某个请求修改了这个变量i， 则在别的请求中能读到这个变量i修改的内容。若是在@Controller之前加上@Scope（"prototype"）就可以改变单例模式到多例模式
* 多个线程请求同一个controller类中的同一个方法， 这俩个线程是互不相干的


# 待续
