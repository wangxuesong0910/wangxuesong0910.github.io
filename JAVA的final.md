---
title: "JAVA的final"
date: 2020-06-16T17:10:55+08:00
draft: true
---

# final

1. final修饰基本类型变量；该变量变成常量。
2. final修饰引用类型变量，该变量地址值固定，但是地址值里面的内容可以发生改变
3. 一个既是static又是final的域只占据一段不能改变的存储空间
4. 类中所有的private方法都**隐式地**指定为final的
5. 当父类中含有final修饰的基本类型变量，继承父类的子类同时继承父类的

## 空白final

* 意义：被声明为final但又未给定初值的域
* 无论什么情况，编译器都会确保final在使用前必须被初始化
* 一个类中的final域可以做到根据对象而有所不同，但又保持其恒定不变的特性

```java
class Poppet{
    private int i;
    Poppet(int ii){
        ii = i;
    }
}
public class BlankFinal {
    //初始化final
    private final int i = 0;

    //空白final
    private final int j;

    //空白final必须在构造器内完成初始化
    public BlankFinal() {
        //初始化空白final
        j = 1;
        //初始化空白final引用
        Poppet p = new Poppet(1);
    }

    public BlankFinal(int j) {
        //初始化空白final
        this.j = j;
        //初始化空白final引用
        Poppet p = new Poppet(j);
    }

    public static void main(String[] args) {
        new BlankFinal();
        new BlankFinal(47);
    }
}

```

* 必须在域的**定义处**或者每个**构造器中**表达式对final进行赋值，这正是final域在使用前总是被初始化的原因所在

## final方法

* 原因一：把方法锁定，以防止任何**继承类**修改它的含义，并且**不会被覆盖**
* 原因二：效率

