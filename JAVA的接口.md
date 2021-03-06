---
title: "JAVA的接口"
date: 2020-06-27T18:21:39+08:00
draft: true
---

# 接口

> 接口就是一种公共的标准规范，只要符合标准规范，大家都可以通用，java的接口更多的体现在对行为的抽象

### 特点

* 接口用关键字interface修饰
* 类实现接口用implements
* 接口不能实例化
  * 接口实例化参照多态，通过实现类对象实例化，这叫接口多态
  * 多态的形式：具体类多态，抽象类多态，接口多态
  * 多态的前提：继承或实现，有方法重写；有

* 接口的实现类
  * 要么重写接口中的所有抽象方法
  * 要么是抽象类

***

### 接口的成员特点

* 成员变量
  * 只能是敞亮
  * 默认修饰符：public static final

* 构造方法
  * 接口没有构造方法，因为接口主要是对行为进行抽象的，没有具体存在，一个类如果没有父类，默认继承自Object类

* 成员方法
  * 只能是抽象方法
  * 默认修饰符：public abstract
  * 关于接口：JDK8以后有一些新特性

***

### 抽象类和接口的区别

* 成员区别
  * 抽象类：变量、常量；有构造方法，抽象方法，非抽象方法
  * 接口：常量；抽象方法

* 关系区别
  * 类与类：继承，单继承
  * 类与接口：实现，可以单实现，也可多实现
  * 接口与接口：继承，单继承，多继承

* 设计理念的区别
  * 抽象类：对类抽象，包括属性行为
  * 接口：对行为抽象，主要是行为

> 抽象类是对事物的抽象，接口是对行为的抽象

