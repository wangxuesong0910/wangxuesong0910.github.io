---
title: "JAVA的Collection"
date: 2020-07-04T15:43:44+08:00
draft: true
---

# Collection

## 集合类的体系结构

![1593849876272](.\1593849876272.png)

## Collection集合概述和使用

### Collection集合概述

* 是单例集合的顶层接口，他表示一组对象，这些对象也称为Collection的元素
* JDK不提供此接口的任何直接实现，它提供更具体的子接口（如Set和List）实现

### 创建Collection集合的对象

* 多态的方式
* 具体的实现类ArrayList

### Collection集合常用方法

| 方法名                     | 说明                               |
| -------------------------- | ---------------------------------- |
| boolean add(E e)           | 添加元素                           |
| boolean remove(Object o)   | 从集合中移除指定的元素             |
| void clear()               | 清空集合中的元素                   |
| boolean contains(Object o) | 判断集合中是否存在指定的元素       |
| boolean isEmpty()          | 判断集合是否为空                   |
| int size()                 | 集合的长度，也就是集合中元素的个数 |

### Collection集合的遍历

* itertor：迭代器，集合的专用遍历方式
  * itertor<E> itertor()：返回此集合中元素的迭代器，通过集合的itertor方法得到
  * 迭代器是通过集合的itertor()方法得到的，所以我们说它是依赖于集合而存在的

* itertor中的常用方法
  * E next()：返回迭代中的下一个元素
  * boolean hasNext()：如果迭代具有更多元素，则返回true

```java
public class Demo{
  public static void main(String[] args){
    Collection<String> c = new ArrayList<String>():
    
    c.add("aa");
    c.add("bb");
    c.add(String.valueOf(11));
    
    Itertor<String> it = c.itertor();
    /*
    System.out.println(it.next());
    System.out.println(it.next());
    System.out.println(it.next());
    System.out.println(it.next());//引发NOSuchElementException:表示请求的元素不存在
    */
      while(it.hasNext()){//判断是否具有更多元素
          System.out.println(it.next());
      }
  }
}
```

***

### List集合概述和特点

* List集合概述
  * 有序集合（也称为有序列），用户可以精确控制列表中每个元素的插入位置，用户可以通过整数索引访问元素并搜索。列表中的元素。

* List集合特点
  * 有序：存储和取出的元素顺序一致
  * 可重复：存储的元素可重复

#### List集合特有方法

| 方法名                        | 说明                                   |
| ----------------------------- | -------------------------------------- |
| void add(int index,E element) | 在此集合中的指定位置插入指定的元素     |
| E remove(int index)           | 删除指定索引处的元素，返回被删除的元素 |
| E set(int index,E element)    | 修改指定索引处的元素，返回被修改的元素 |
| E get(int index)              | 返回指定索引处的元素                   |

#### 并发修改异常

* 并发修改异常
  * ConcurrentModificationException

* 产生原因
  * 迭代器遍历的过程中，通过集合对象修改了集合中元素的长度，造成了迭代器回去元素中判断预期修改值和实际修改值不一致

* 解决方案
  * for循环遍历，用集合对象做相应操作

> 详情：[bubudeai.xyz]()之并发修改异常篇

#### 列表迭代器

* ListItertor：列表迭代器
  * 通过List集合的listItertor()方法得到，所以说它是List集合他有的迭代器
  * 用于允许程序员沿任一方向遍历列表的列表迭代器，在迭代期间修改列表，并获取列表中迭代器的当前位置

##### ListItertor中的常用方法

| 方法名              | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| E next()            | 返回迭代中的下一个元素                                       |
| boolean hasNext()   | 如果迭代具有更多元素，则返回true                             |
| E previous()        | 返回列表中的上一个元素                                       |
| boolean hasPrevious | 如果此列表迭代器在相反方向遍历列表时具有更多元素，则返回true |
| void add(E e)       | 将指定的元素插入列表                                         |

> 使用listItertor进行add操作可避免并发修改异常

```java
public static void main(String[] args){
  List<String> s = new ArrayList<>();
  
  s.add("aa");
  s.add("bb");
  s.add("cc");
  
  ListItertor<String> si = s.listItertor();
  
  while(si.hasnext()){
    String next = si.next();
    
    if(next.equals("aa")){
      si.add("dd");//此处如果使用s.add("dd");仍会报并发修改异常，因为其使用的仍然是Itertor的add()
    }
  }
}
```

***

#### 增强for循环

* 增强for：简化数组和Collection集合的遍历
  * 实现Itertor接口的类允许其对象成为增强型for语句的目标
  * 它是JDK5以后出现的，其内部原理是一个Itertor迭代器

* 增强for的格式

```java
for（元素数据类型 变量名：数组或者Collection集合）{
  //在此处使用变量即可，该变量就是元素
}

//以上面的为例
for(String ss:s ){
    System.out.println(ss);
}
```

***

#### List集合子类特点

* List集合常用子类：ArrayList，LinkedList
  * ArrayList：底层数据结构是数组，查询快，增删慢
  * LinkedList：底层数据结构是链表，查询慢，增删快
    * LinkedList能够直接实现栈的所有功能的方法，可以直接将LinkedList作为栈使用

#### LinkedList集合的特有功能

| 方法名                    | 说明                             |
| ------------------------- | -------------------------------- |
| public void addFirst(E e) | 在该列表开头插入指定的元素       |
| public void addLast(E e)  | 将指定的元素追加到此列表的末尾   |
| public E getFirst()       | 返回此列表中的第一个元素         |
| public E getLast()        | 返回此列表的最后一个元素         |
| public E removeFirst()    | 从此列表中删除并返回第一个元素   |
| public E removeLast()     | 从此列表中删除并返回最后一个元素 |

***

###  Set集合概述和特点

* Set集合特点
  * 不包含重复元素的集合
  * 没有带索引的方法，所以不能使用不同for循环遍历
  * Set最常被使用的是测试归属性，(查找就成为了Set中最重要的操作)
  * Set具有与Collection完全一样的接口，因此没有额外的功能，实际上Set就是Collection，只是行为不同（继承与多态思想的典型应用：表现不同的行为），Set是基于对象的值来确定归属性

```java
public class SetOfInteger {
    public static void main(String[] args) {
        Random random = new Random(47);
        HashSet<Integer> inset = new HashSet<>();
        for (int i = 0; i <10000 ; i++) {
            inset.add(random.nextInt(30));
        }
        System.out.println(inset);
        System.out.println(inset.size());
    }
}
```

#### 哈希值(单独一篇写HashCode())

> 哈希值：是JDK根据对象的地址或者字符串或者数字算出来的int类型的数值

* Object类中有个方法可以获取对象的哈希值
  * public int hashCode()：返回对象的哈希码值

> 值得注意的是：同一个对象多次调用hashCode()方法返回的哈希值是相同的（默认情况下），不同对象是不同的，在没有重写hashCode()方法的情况下，想要相同哈希值就重写hashCode()方法

> Tips: 字符串重写了hashCode()方法

#### HashSet集合概述和特点

* HashSet集合特点
  * 底层数据结构是哈希表
  * ***对集合的迭代顺序不做任何保证***，也就是说不保证存储和取出的元素顺序一致
  * 没有带索引的方法，所以不能使用普通for循环遍历
  * 由于是Set集合，所以是不包含重复元素的集合
  * HashSet使用散列函数存储元素

***

#### LinkedHashSet集合概述和特点

* LinkedHashSet集合特点
  * 哈希表和链表实现的Set接口，具有可预测的迭代次序
  * 由链表保证元素有序，也就是说元素的存储和取出的顺序是一致的
  * 由哈希表保证元素唯一，也就是说没有重复的元素

***

#### TreeSet集合概述和特点

* TreeSet集合特点
  * 元素有序，这里的顺序不是指存储和取出的顺序，而是按照一定规则进行排序，具体排序方式取决于构造方法
    * TreeSet()：根据其元素的自然排序进行排序
    * TreeSet（Comparator comparator）：根据指定的比较器进行排序
  * TreeSet将元素存储在红-黑树数据结构中
  
* * 没有带索引的方法，所以不能使用普通for循环遍历
  * 由于是Set集合，所以是不包含重复元素的集合

##### 自然排序Comparable的使用

```java
//接口Comparable：对实现它的每个类的对象强加一个整体排序，这个排序被称为类的自然排序，类的comparaTo方法称为自然比较方法
//重写comparaTo方法使用示例
//此方法判断比较最好有主次
public int comparaTo(Student s){
  //比较学生年龄
  int i = this.age - s.age;//this指现添加进来的值，参数s指原有的值，此结果意在返回正数
  int i = s.age - this.age;//此结果意在返回负数，升序this放在前面，降序this放在后面
  
  //当年龄相同时，次要比较学生姓名
  int i1 = i==0?this.name.comparaTo(s.name):i;//学生姓名为String类型，实现了Comparable接口
  return i1;//返回结果i为正数则认为是按照存储顺序输出（升序），负数则是按照存储顺序输出（降序），0则是相同元素
}
```

##### 比较器排序Comparator的使用

* 用TreeSet集合存储自定义对象，带参构造方法使用的是比较器排序对元素进行排序的
* 比较器排序，就是让集合构造方法接收Comparator的实现类对象，重写compare(T  o1 , T  o2)方法
* 重写方法时一定要注意排序规则必须按照要求的主要条件和次要条件来写

```java
TreeSet<Student> ts = new TreeSet<>(new Comparator<Student>(){
  @Override
  public int compare(Student s1, Student s2){
   int i = s1.getAge() - s2.getAge();//主要比较条件
   int i1 = i == 0? s1.getName().compareTo(s2.getName()):i;//次要比较条件
   
   return i1;
  
  }
 });

```

***

## Collections概述和使用

* Collections类的概述
  * 是针对集合操作的工具类

* Collections类的常用方法
  * public static <T extends Comparable<? super T>> void sort(List<T> list)：将指定的列表升序排序
  * public static void reverse(List<?>list)：反转指定列表中元素的顺序
  * public static void shuffle(List<?>list)：使用默认的随机源随机排列指定的列表
  * addAll(Collection<? super T> c,  T... elements)：将所有指定的元素添加到指定的集合

```java
public class Demo07 {
    public static void main(String[] args) {
        HashMap<Integer, String> ism = new HashMap<>();
//        ArrayList<String> strings = new ArrayList<>();

        ArrayList<Integer> integers = new ArrayList<>();
        String[] hs = {"♣", "♦", "♥", "♠"};
        String[] poke = {"2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"};
        int i = 0;
        for (String s : hs) {
            for (String s1 : poke) {
                ism.put(i, s + s1);
                integers.add(i);
                i++;
            }
        }

        System.out.println(i);


        ism.put(i, "大王");
        integers.add(i);
        i++;
        ism.put(i, "小王");
        integers.add(i);
        System.out.println(ism);
        //洗牌
        Collections.shuffle(integers);
        //发牌
        TreeSet<Integer> ts1 = new TreeSet<>();
        TreeSet<Integer> ts2 = new TreeSet<>();
        TreeSet<Integer> ts3 = new TreeSet<>();
        TreeSet<Integer> ts4 = new TreeSet<>();

        for (int j = 0; j < integers.size(); j++) {

            if (j >= integers.size() - 3) {
                ts4.add(integers.get(j));

            } else if (j % 3 == 0) {
                ts1.add(integers.get(j));

            } else if (j % 3 == 1) {
                ts2.add(integers.get(j));

            } else if (j % 3 == 2) {
                ts3.add(integers.get(j));

            }
        }

        lookpk("wxs",ts1,ism);
        System.out.println(ts4);

    }

    public static void lookpk(String name,TreeSet<Integer> tsi,HashMap<Integer,String> hashMap){
        System.out.print(name+"的牌是：");
        for (Integer i :tsi){
            String s = hashMap.get(i);
            System.out.print(s+" ");
        }
        System.out.println();
    }
}

```

