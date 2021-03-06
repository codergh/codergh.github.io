---
layout: post
title: Java 面试总结
tags:
  - 面试
  -
category: java
---
生命不息， 学习不止，愿每次面试都能指引我进步的方向
<!--more-->
# Java 面向对象的开发语言有什么特点
面向对象开发有三大特性，封装，继承和多态，这几个特点为Java带来了几个优势。

* 模块化，便于维护
* 复用性，减少代码的冗余
* 增强代码的灵活性和可靠性
* 便于理解，提高开发效率

封装：对象提供了隐藏内部属性和行为的能力，只曝漏一些公开的方法， 通过这些方法可以对对象数据进行修改。下面列出一些封装的好处：

 * 通过隐藏对象的属性来保护对象的状态
 * 通过限制改变对象的方法来减少对象间的不良交互，提高模块化
 * 提高代码的可用性和可维护性， 因为对象的行为可以被单独的改变或者扩展

继承：是类与类之间的一种关心，继承提供了子类对象获取父类对象属性和行为的能力，提高代码的复用能力。可以在不修改父类的前提下添加子类新的行为。

多态：同一个类的不同对象调用相同的方法产生不同的行为。多态的目的是解除同类型的耦合关系

# 什么是Java虚拟机
Java虚拟机是一个提供Java字节码运行的一套虚拟环境，Java虚拟机的存在使跨平台产生了可能。

# static关键字
static类型的变量会在编译时进行绑定， 因此static类型的方法调用非static变量在编译时会报错， 因为非static变量是运行时变量， 而static方法则是在编译时进行绑定

# 线程
## 创建线程的几种方法

 * 继承Thread类
 * 是想Runable借口
 * 获取框架获取线程池，通过线程池创建线程

## 同步方法与同步代码块
都是通过synchronized这个关键字来进行定义的， 不同的是一个是方法一个是代码块， 但是线程获取锁的时候都是对象锁




