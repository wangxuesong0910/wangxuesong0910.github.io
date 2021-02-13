---
title: "API学习(实时更新)"
date: 2020-06-29T15:54:49+08:00
draft: true
---

# JAVA的API

## Math类概述

> Math 包含执行基本数字运算的方法

* 没有构造方法，如何使用类中的成员呢？
  * 看类的成员是否都是静态的，如果是，通过类名就可以直接调用

### Math类的常用方法

| 方法名                                      | 说明                                           |
| ------------------------------------------- | ---------------------------------------------- |
| public static int abs(int a)                | 返回参数的绝对值                               |
| public static double ceil(double a)         | 返回大于或等于参数的最小double值，等于一个整数 |
| public static double floor(double a)        | 返回小于或等于参数的最大double值，等于一个整数 |
| public static int round(float a)            | 按照四舍五入返回最接近参数的int                |
| public static int max(int a , int b)        | 返回两个int值中的较大值                        |
| public static int min (int a , int b)       | 返回两个int值中的较小值                        |
| public static double pow(double a,double b) | 返回a的b次幂的值                               |
| public static double random()               | 返回值为double 的正值，[0.0,1.0)               |

***

## System类概述

> System包含几个有用的类字段和方法，他不能被实例化

### System类的常用方法

| 方法名                                 | 说明                                       |
| -------------------------------------- | ------------------------------------------ |
| public static void exit(int status)    | 终止当前运行的JAVA虚拟机，非零表示异常终止 |
| public static long currentTimeMillis() | 返回当前时间（以毫秒为单位）               |

***

## Object类概述

> Object是类层次结构的根，每个类都可以将Object作为超类，所有类都直接或者间接的继承自该类
>
> 构造方法：public Object()

* 为什么说子类的构造方法默认访问的是父类的无参构造方法？
  * 因为他们的顶级父类中只有无参构造方法

### Object类的常用方法

| 方法名                            | 说明                                             |
| --------------------------------- | ------------------------------------------------ |
| public String toString()          | 返回对象的字符串表示形式，建议所有子类重写该方法 |
| public boolean equals(Object obj) | 比较对象是否相等，默认比较地址，重写可以比较内容 |

***

## Arrays类概述

> Arrays类包含用于操作数组的各种方法

### Arrays类的常用方法

| 方法名                                 | 说明                               |
| -------------------------------------- | ---------------------------------- |
| public static String toString(int[] a) | 返回指定数组的内容的字符串表示形式 |
| public static void sort(int[] a)       | 按照数字顺序排列指定的数组         |

***

## 基本类型包装类

## Integer类概述

> Integer：包装一个对象中的原始类型int的值

### 使用

| 方法名                                  | 说明                                  |
| --------------------------------------- | ------------------------------------- |
| public Integer(int value)               | 根据int值创建Integer对象（过时）      |
| public Integer(String s)                | 根据String值创建Integer对象（过时）   |
| public static Integer valueOf(int i)    | 返回表示指定的int值得Integer实例      |
| public static Integer valueOf(String s) | 返回一个保存指定值得Integer对象String |

#### int 和 String的相互转换

* int转换为String
  * public static String valueOf(int i):返回int参数的字符串表示形式，该方法是String类中的方法

* String转换为int
  * public static int parseInt(String s):将字符串解析为int类型，该方法是Integer类中的方法

***

## Date类概述

> Date代表了一个特定的时间，精确到毫秒

### Date类的常用方法

| 方法名                         | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| public Date()                  | 分配一个Date对象，并初始化，以便它代表它被分配的时间，精确到毫秒 |
| public Date(long date)         | 分配一个Date对象，并将其初始化为表示从标准基准时间起指定的毫秒数 |
| public long getTime()          | 获取的是日期对象从1970年1月1日零时到现在的毫秒值             |
| public void setTime(long time) | 设置时间，给的是毫秒值                                       |

#### SimpleDateFormat类概述

> SimpleDateFormat是一个具体的类，用于以区域设置敏感的方式格式化和解析日期
>
> Tips：日期和时间格式由日期和时间模式字符串自定，在日期和时间模式字符串中，从 'A' 到 'Z'  以及从 'a' 到 'z'  引号的字母被解释为表示日期或时间字符串的组件模式字母

##### SimpleDateFormat的构造方法

| 方法名                                  | 说明                                                 |
| --------------------------------------- | ---------------------------------------------------- |
| public SimpleDateFormat()               | 构造一个SimpleDateFormat，使用默认模式和日期格式     |
| public SimpleDateFormat(String pattern) | 构造一个SimpleDateFormat使用给定的模式和默认日期格式 |

##### SimpleDateFormat格式化和解析日期

* 格式化（从  Date  到  String）
  
* public final String format(Date date): 将日期格式化成日期/时间字符串 
  
    ```java
    //格式化:从Date到String
    Date date = new Date();
    System.out.println(date.getTime());//获取当前时间
    
    SimpleDateFormat sd = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    
    String format = sd.format(date);
    System.out.println(format);
    ```
  
    
  
* 解析（从 String  到  Date）
  
  * public Date parse(String source): 从给定字符串的开始解析文本以生成日期
  
    ```java
    //解析：从String到Date
    Date date = new Date();
    
    String s = "2000-01-01 11:11:12";
    
    SimpleDateFormat sd = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    
    Date d = sd.parse(s);//此处需要处理异常
    System.out.println(d);
    ```
  
    

***

#### 日期类

##### Calendar类概述

> Calendar 为某一时刻和一组日历字段之间的转换提供了一些方法，并为操作日历字段提供了一些方法

* Calendar提供了一个类方法getInstance用于获取Calendar对象，其日历字段已使用当前日期和时间初始化
  * Calendar rightNow = Calendar.getInstance();

##### Calendar的常用方法

| 方法名                                             | 说明                                                   |
| -------------------------------------------------- | ------------------------------------------------------ |
| public int get(int field)                          | 返回给定日历字段的值                                   |
| public abstract void add(int field, int amount)    | 根据日历的规则，将指定的时间量添加或减去给定的日历字段 |
| public final void set(int year,int month,int date) | 设置当前日历的年月日                                   |

