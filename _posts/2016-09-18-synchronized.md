---
layout: post
title: 'Synchronized关键字'
tags:
  - 同步
  - 数据一致性
category: java
---
Synchronized是Java语言的一个关键字， 当它被用来修饰一个方法或者代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码
<!--more-->

# synchronized方法
synchronized方法控制对类成员变量的访问，每个类实例都会对应一个锁，每个线程都必须获得对应实例对象的锁才能执行，否则阻塞该线程， 如果方法一旦执行， 则该线程独占实例锁， 直到该方法返回时才释放实例锁，此后被阻塞的线程才能获得实例锁，恢复函数执行。

弱点： 若将一个大的方法声明为synchronized，将大大影响执行效率， 典型的就是将线程类的run方法声明为synchronized

# synchronized块
synchronized块是比synchronized方法更简便的用法， 语法如下：

    synchronized(synObject) {
        ....允许访问控制的代码
    }

要执行`允许访问控制的代码`则该线程必须获得synObject锁， synObject可以是类或者类对象

## synchronized(this)的理解
* 当两个以上的线程访问Object对象的synchronized(this)代码块时， 同一时间内只有一个线程能执行， 另外的线程必须等待当前线程执行完这个代码块后才能执行该代码块。
* 当一个线程在访问Object中的synchronized(this)代码块时， 另外一个线程可以无阻塞的访问非synchronized(this)代码块。
* 当一个线程在访问Object中的synchronized(this)代码块时， 其他线程对Object的其他synchronized(this)代码块的访问也会被阻塞。

# 小结

* 无论是synchronized声明到方法还是代码上，线程取得的锁都是对象锁， 而不是把一段代码或者方法当作是锁
* 每个对象只有一个锁与之相关联
* 实现同步是要很大的系统开销的， 还有可能形成死锁， 所以应该尽量避免使用无谓的同步
