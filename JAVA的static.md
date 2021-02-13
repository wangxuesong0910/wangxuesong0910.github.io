---
title: "JAVA的static"
date: 2020-06-16T17:16:29+08:00
draft: true
---

# static

<span style="color:green">static关键字是静态的意思，可以修饰成员方法，成员变量</span>

* <span style="color:green">static</span>修饰的特点
* 被类的所有对象共享（这也是我们判断是否使用静态关键字的条件）
* 可以通过类名调用，也可以通过对象名调用

***

##  <span style="color:red">static</span>访问特点

1. 非静态的成员方法
   * 能访问静态的成员变量和成员方法
   * 能访问非静态的成员变量和成员方法

2. 静态的成员方法
   * 能访问静态的成员变量和成员方法

总结：<span style="color:red">静态成员方法只能访问静态成员</span>

***

