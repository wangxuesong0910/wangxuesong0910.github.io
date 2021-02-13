---
title: "JAVA的内部类"
date: 2020-06-28T16:42:29+08:00
draft: true
---

# 内部类

> 内部类：就是在一个类中定义一个类；例：在类A的内部定义一个类B，类B就被称为内部类

* 内部类访问特点：
  * 内部类可以直接访问外部类的成员，包括私有
  * 外部类要访问内部类的成员，必须创建对象

```java
public class Outer{
  private int num = 10;
  public class Inner{
  
    public void show(){
      num = 20;
    }
    
  }
  
  public void method(){
    Inner i = new Inner();
    i.show();
  }
}
```

***

### 成员内部类

* 按照内部类在类定义的位置不同，可以分为如下两种形式
  * 在类的成员位置：成员内部类
  * 在类的局部位置：局部内部类

* 成员内部类，外界创建对象使用
  * 格式：外部类名.内部类名 对象名 = 外部类对象.内部类对象;
  * 例：Outer.Inner oi = new Outer().new Inner();

```java
public class Outer{
  private int num = 10;
  /*一般定义为内部类是不希望外界直接访问
  public class Inner{
  
    public void show(){
      System.out.println(num);
    }
    
  }
  */
  
  private class Inner{
  //内部类一般定义为private
    public void show(){
      System.out.println(num);
    }
    
  }
  
  public void method(){
      //外部类访问内部类成员，必须创建对象
    Inner i = new Inner();
    i.show();
  }
}
```

```java
public class Demo{
  public static void main(String[] args){
    /*
    Outer.Inner oi = new Outer().new Inner();
    oi.show();
    */
    
    Outer o = new Outer();
    o.method();
  }
}
```

***

### 局部内部类

> 局部内部类是在方法中定义的类，所以外界是无法直接使用，需要在方法内部创建对象并使用
>
> 该类可以直接访问外部类成员，也可以直接访问方法内的局部变量

```java
public class Outer{
  private int num = 10;
  
  public void method(){
      
    class Inner{
      public void show(){
      System.out.println(num);
      }
    }
    
    Inner i  = new Inner();
    i.show();
  }
    
}
```

```java
public class Demo{
  public static void main(String[] args){
    Outer o = new Outer();
    o.method();
  }
}
```

***

### 匿名内部类

> 前提：存在一个类或者接口，这里的类可以使具体类也可以是抽象类

* 格式：

  ```java
  new 类名或接口名(){
    重写方法;
  };
  
  //例：
  new Inter(){
        public void show(){
            
        }
  };
  ```

  > 本质：是一个继承了该类或者实现了该接口的子类匿名对象

```java
public interface Inter{
  void show();
}
```

```java
public class Outer{
  
  public void method(){
  
  //第一种使用方法
  new Inter(){
    @Override
    public void show(){
      System.out.println("匿名内部类使用");
    }
  }.show();
  
  //第二种使用方法
  Inter i = new Inter(){
    @Override
    public void show(){
      System.out.println("匿名内部类使用");
    }
  };
  i.show();
  
  }
    //第三种使用方法
    public Inter inter(){
    return new Inter() {
        @Override
        public void show() {
            System.out.println("匿名内部类使用");
        }
    };
  }
    
}
```

```java
public class Demo{
  public static void main(String[] args){
    Outer o = new Outer();
    o.method();
  }
}
```

> 实际开发中的应用：当一个方法的返回值类型为接口或类时可以使用匿名内部类，而不必再去创建单独的实现类或子类。

#### 匿名内部类中创建构造器

* 通过实例初始化，能够为匿名内部类创建一个构造器

```java
abstract class Base {
    public  Base(int i){
        System.out.println("Base类的构造器，i ="+ i);
    }

    public abstract void f();
}

public class AnonymousConstructor {
    public static Base getBase(int i){
        return new Base(i){
            {
                System.out.println("内部实例初始化");
            }
            public void f(){
                System.out.println("anony内的f()");
            }
        };
    }

    public static void main(String[] args) {
        Base base = getBase(47);
        base.f();
    }
}

```

#### 匿名内部类的参数为final的条件

* 在匿名内部类中，希望使用一个在其外部定义的对象，编译器要求其参数引用时final的

```java
public class Parcel10 {
    public Destination destination(final String dest,final float price){
        return new Destination() {
            private int cost;
            {
                cost = Math.round(price);
                if (cost > 100){
                    System.out.println("Over budget:没有预算");
                }
            }
            private String  lable = dest;
            @Override
            public String readLable() {
                return lable;
            }
        };
    }

    public static void main(String[] args) {
        Parcel10 p = new Parcel10();
        Destination wxs = p.destination("wxs", 101.395F);
    }
}

```

* 对于匿名类而言，实例初始化的实际效果就是构造器，仅有一个：所以不能重载实例初始化方法
* 匿名内部类与正规的继承相比有些受限，因为匿名内部类既可以扩展类，也可以实现接口，但是不能两者兼备，如果实现接口只能实现一个接口

### 嵌套类

* 不需要内部类对象与其外围类对象之间有联系，可以将内部类声明为static
* static应用于内部类的含义：普通的内部类对象隐式地保存了一个引用，指向创建它的外围类对象

* 嵌套类意味着
  * 要创建嵌套类的对象，并不需要外围类的对象
  * 不能从嵌套类的对象中访问非静态的外围类对象

* ps：普通内部类的字段与方法，只能放在类的外部层次上，所以普通内部类不能有static数据和static字段，也不能包含嵌套类，但是嵌套类可以包含这些东西

```java
public class Parcel11 {
    private static class ParcelContents implements Contents{
        private int i = 11;
        @Override
        public int value() {
            return i;
        }
    }

    protected static class ParceDestination implements Destination{
        private String lable;

        private ParceDestination(String lable) {
            this.lable = lable;
        }

        @Override
        public String readLable() {
            return lable;
        }
        public static void f(){}
        static int x= 10;
        static class AnotherLevel{
            public static void f(){}
            static int x = 10;
        }
    }

    public static Destination destination(String s){
        return new ParceDestination(s);
    }

    public static Contents contents(){
        return new ParcelContents();
    }

    public static void main(String[] args) {
        Contents c = contents();
        Destination d = destination("wxs");
    }
}

```

### 为什么需要内部类

1. 内部类提供了某种进入其外围类的窗口
   * 内部类继承自某个类或实现某个接口，内部类的代码操作创建它的外围类的对象

2. 内部类最吸引人的原因：
   * 每个内部类都能独立地继承自一个(接口的)实现，所以无论外围类是否已经继承了某个(接口的)实现，对于内部类都没有影响

3. 内部类允许继承多个非接口类型(译注：类或抽象类)

```java
interface A{}
interface B{}

class X implements A,B{}

class Y implements A{
    B makeB(){
        return new B(){};
    }
}
public class MultiInterfaces {
    static void takesA(A a){}
    static void takesB(B b){}

    public static void main(String[] args) {
        X x = new X();
        Y y = new Y();
        takesA(x);
        takesA(y);
        takesB(x);
        takesB(y.makeB());
    }
}
```

#### 内部类实现多重继承

* 如果你拥有抽象的类或具体的类，而不是接口，只能使用内部类才能实现多重继承

```java
class D{}
abstract class E{}

class Z extends D{
    E makeE(){
        return new E(){};
    }
}

public class MultiImplementation {
    static void takesD(D d){}
    static void takesE(E e){}

    public static void main(String[] args) {
        Z z = new Z();
        takesD(z);
        takesE(z.makeE());
    }
}

```

#### 内部类的一些其他特性

1. 内部类可以有多个实例，每个实例都有自己的状态信息，并且与其外围类对象的信息相互独立
2. 在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或继承同一个类
3. 创建内部类对象的时刻不依赖外围类对象的创建9
4. 内部类并没有令人迷惑的"is - a"关系，就是一个独立的实体