---
title: "JAVA的泛型"
date: 2020-07-10T16:47:55+08:00
draft: true
---

# 泛型

> 泛型：是JDK5中引入的特性，它提供了编译时类型安全检测机制，该机制允许在编译时检测到非法的类型
>
> 它的本质是***参数化类型***，也就是说所操作的数据类型被指定为一个参数
>
> 一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参，那么参数化类型怎么理解呢？
>
> 顾名思义，就是讲类型由原来的具体的类型参数化，然后在使用/调用时传入具体的类型
>
> 这种参数类型可以在类、方法和接口中，分别被称为泛型类、泛型方法和泛型接口

* 泛型定义格式：
  * <类型>：指定一种类型的格式。这里的类型可以看成是形参
  * <类型1，类型2...>：指定多种类型的格式，多种类型之间用逗号隔开。这里的类型可以看成是形参
  * 将来具体调用时候给定的类型可以看成是实参，并且实参的类型只能是引用数据类型

* 泛型的好处：

  * 把运行时期的问题提前到编译期间

  * 避免了强制类型转换

    ***

    

* 泛型类的定义格式
  * 格式：修饰符 class 类名<类型>{}
  * 范例：public class Generic<T>{}
    * 此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型

```java
public class Generic<T>{
  private T t;
  
  public T getT(){
    return t;
  }
  
  public void setT(T t){
    this.t=t;
  }
}
```

```java
public class Demo{
  public static void main(String[] args){
    Generic<String> g = new Generic<String>();
    
    g.setT("www");
  }
}
```

***

## 泛型方法

```java
public class Generic{
    
    //使用方法重载，传递不同类型的参数需要多个不同方法
  public void show(String s){
    System.out.println(s);
  }
  
  public void show(Integer i){
    System.out.println(i);
  }
  
  public void show(Boolean b){
    System.out.println(b);
  }
}
```

```java
public class Generic{
//使用泛型类解决多个方法问题，但是实例化对象还是需要指明具体类型，这样就需要实例化多个对象
//那么有没有办法改进呢？
  public void show(T t){
    System.out.println(t);
  }
}
```

```java
public class Generic{
//使用泛型方法可以解决上面遇到的需要实例化多个对象并指明具体参数类型的问题
//泛型方法可以在调用方法时再明确对象参数类型
public <T> void show(T t){//通过对象调用方法时，传递什么类型的参数，就是什么类型。
  System.out.println(t);
}
}
```

***

## 泛型接口

* 泛型接口定义格式：
  * 格式：修饰符 interface 接口名<类型>{}
  * 范例：public interface Generic<T>{}

```java
public interface Generic<T>{
    //定义一个泛型接口，写一个抽象方法show()
  void show(T t);
}
```

```java
public class GenericImpl<T> implements Generic<T>{
  //编写泛型接口的实现类XXXImpl，重写show（）方法
  @Override
  public void show(T t){
    System.out.println(t);
  }
}
```

```java
public class Demo{
  public static void main(Strings[] args){
    GenericImpl<String> gs = new GenericImpl<String>();
    
    gs.show("www");
    gs.show(11);//报错
  }
}
```

***

## 类型通配符

* 为了表示各种泛型List的父类，可以使用类型通配符
  * 类型通配符：***<?>***
  * List<?>：表示元素类型位置的List，它的元素可以匹配任何类型
  * 这种带通配符的List仅表示它是各种泛型List的父类，并不能把元素添加到其中

* 如果说我们不希望List<?> 是任何泛型List的父类，只希望它代表某一类泛型List的父类，可以使用通配符的上限
  * 类型通配符上限：<？ extends 类型>
  * List<? extends Number>：它表示的类型是Number或者其子类型

* 除了可以指定类型通配符的上限，我们也可以指定类型通配符的下限
  * 类型通配符下限：<? super 类型>
  * List<? super Number>：它表示的类型是Number或者其父类型

```java
public class Demo{
  public static void main(String[] args){
    
    List<?> li = new ArrayList<Object>();
    List<?> li = new ArrayList<String>();
    
      //类型通配符上限
    List<? extends Number> li = new ArrayList<Integer>();
      
    //类型通配符下限
    List<? super Integer> li = new ArrayList<Number>();
  }
}
```

***

## 可变参数

* 可变参数又称参数个数可变，用作方法的形参出现，那么方法参数个数就是可变的了
  * 格式：修饰符 返回值类型 方法名 （数据类型...变量名）｛｝
  * 范例：public static int sum(int...a){}

* 可变参数注意事项
  * **这里的变量其实是一个数组**
  * 如果一个方法有多个参数，包含可变参数，可变参数要放最后

```java
public class Demo{
  public static void main(String[] args){
    System.out.println(sum(11,22,33,44));//此处11的值为变量b
  }
  
  public static int sum(int b,int...a){//定义一个带可变参数的方法，包含多个参数，这里的int...a其实是一个数组
    int count = 0;
    for(int i: a){
      count += i;
    }
    return count;
  }
}
```

### 可变参数的使用

* Arrays工具类中有一个静态方法
  * public static <T> List <T> asList(T...a)：返回由指定数组支持的固定大小的列表
  * 返回的集合不能做增删操作，可以做修改操作

* List接口中有一个静态方法：
  * public static <E> List <E> of(E...elements)：返回包含任意数量元素的不可变列表
  * 返回的集合不能做增删改操作（JDK9以后的特性）

* Set接口中有一个静态方法：
  * public static <E> Set<E> of(E...elements)：返回一个包含任意数量元素的不可变集合
  * 在给元素的时候，不能给重复的元素
  * 返回的集合不能做增删操作，没有修改的方法