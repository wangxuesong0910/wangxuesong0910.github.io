---
title: "JAVA的多态"
date: 2020-06-18T15:20:38+08:00
draft: true
---

# 多态

* <span style="color:green">多态的前提和体现</span>
  * 有继承(extends)/实现(implements)关系
  * 有方法重写
  * 有父类引用指向子类对象

* <span style="color:green">多态中成员访问特点</span>

  * <span style="color:red">成员变量：</span>编译看左边，执行看左边

  * <span style="color:red">成员方法：</span>编译看左边，执行看右边

  * > 为什么？
    >
    > 因为成员方法有重写，成员变量没有

***

<span style="background:#00bf00">具体例子如下：</span>

```java
public class Animal{
  public void eat(){
  System.out.println("动物吃东西");
  }
}
```

```java
public class Cat extends Animal{
  public void eat(){
  System.out.println("猫吃鱼");
  }
}
```

```java
public class AnimalOperator{
  public void userAnimal(Animal ani){
  //Animal ani = new (子类)
  //父类new子类
    ani.eat();
  }
}
```

```java
public class Demo{
  public static void main(String[] args){
    AnimalOperator ao = new AnimalOperator;
    Cat c = new Cat();
    ao.userAnimal(c);
  }

}
```

***

### <span style="color:red">多态</span>的好处和弊端

* <span style="background:#00bf00">多态的好处：</span>
  * 提高程序的 扩展性。
  * 定义方法的时候，使用父类型作为参数，将来在使用的时候，使用具体的子类型参与操作

* <span style="background:#00bf00">多态的弊端：</span>
  * 不能使用子类的特有功能。

***

### <span style="color:red">多态</span>的转型

```java
public class Animal{
  public void eat(){
    System.out.println("动物吃东西");
  }
}
```

```java
public class Cat extends Animal{
  public void eat(){
    System.out.println("猫吃鱼");
  }
  
  public void play(){
    System.out.println("猫玩耍");
  }
}
```

```java
public class Demo{
  public static void main(Strings[] args){
    //向上转型
    Animal a = new Cat();
      a.eat();
      //a.play();报错：父类中没有play方法，编译看左边
    //向下转型
    Cat c = (Cat)a;
      c.eat();
      c.play();
  }
}
```

