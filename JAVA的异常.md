---
title: "JAVA的异常"
date: 2020-07-04T08:42:33+08:00
draft: true
---

# JAVA的异常

## 异常概述

> 异常：程序出现不正常情况

* 体系

  ![1593823954517](.\1593823954517.png)

* Error：严重问题
* Exception：称为异常类，它表示程序本身可以处理的问题
  * RuntimeException：在编译期没有检查出来，出现问题后，需要我们回来修改代码
  * 非RuntimeException: 编译期就必须处理，否则程序不能通过编译，更不能正常运行

***

## JVM的默认处理方案

> 如果程序出现问题，我们没做任何处理，最终JVM会做默认的处理

* 把异常名称，异常原因以及出现异常的位置等信息输出在控制台
* 程序停止执行

***

## 异常处理

### 异常处理之try...catch...

* 格式：

  ```java
  try{
      可能出现异常的代码
  } catch(异常类名 变量名){
    异常的处理代码
  }
  ```

  > 执行流程：
  >
  > 程序从try里面的代码开始执行
  >
  > 出现异常，会自动生成一个异常类对象，该异常类对象将被提交给java运行时系统
  >
  > 当java运行时系统接收到异常对象时，会到catch块中去找匹配的异常类，找到后进行异常的处理
  >
  > 执行完毕之后，程序还可以继续往下执行

***

### Throwable的成员方法

| 方法名                        | 说明                            |
| ----------------------------- | ------------------------------- |
| public String getMessage()    | 返回此throwable的详细消息字符串 |
| public String toString()      | 返回此可抛出的简短描述          |
| public void printStackTrace() | 把异常的错误信息输出在控制台    |

### 编译时异常和运行时异常的区别

> Java中的异常被分为两大类：编译时异常和运行时异常，也叫受检异常和非受检异常
>
> 所有的RuntimeException类及其子类被称为运行时异常，其他的异常都是编译时异常

* 编译时异常：必须显示处理，否则程序会发生错误，无法通过编译
* 运行时异常：无需显示处理，也可以和编译时异常一样处理

***

### 异常处理之throws

虽然try...catch...可以对异常进行处理，但并不是所有的情况我们都有权限进行异常的处理

有些时候可能出现的异常是我们处理不了的，这个时候该怎么办呢？

java提供了throws的处理方案

* 格式：

  ```java
  throws 异常类名;
  //这个格式是跟在方法后面的
  ```

  

* 编译时异常必须要进行处理，两种处理方案：try...catch...或者throws，如果采用throws这种方案，将来谁调用谁处理
* 运行时异常可以不处理，出现问题后，需要我们回来修改代码

***

### 自定义异常

* 格式：

  ```java
  public class 异常类名 extends Exception{
    无参构造
    带参构造
  }
  ```

* 范例：

  ```java
  public class ScoreException extends Exception{
      public ScoreException(){}
      public ScoreException(String  message){
        super(message);
      }
  }
  ```

  

* 实例详解：

  ```java
  public class ScoreException extends Exception{
  //自定义异常类
    public ScoreException(){}
      public ScoreException(String message){
        super(message);
      }
  }
  ```

  ```java
  //定义一个使用异常类的类
  public class Teacher{
    public void checkScore(int score) throws Exception{//此处需要抛出异常，如果是运行时异常可选择抛出或者不抛出
      if(score<0 || score>100){
        throw new ScoreException();//如果不符合条件，抛出我们自定义的异常，也可以使用我们自定义的带参构造方法，给出详细的错误信息提示
      }else{
        //执行语句
      }
    }
  }
  ```

  ```java
  public class Demo{
    public static void main(Strings[] args){
      Scanner sc = new Scanner(System.in);
      int score = sc.nextInt();
      
      Teacher t = new Teacher();
      //t.checkScore(score);这么写会出现异常，需要处理，可以用try...catch...来处理
      try{
        t.checkScore(score)
      }catch(ScoreException e){
        e.printStackTrace();
      }
    }
  }
  ```

  

### throws 和 throw 的区别

| throws                                       | throw                          |
| -------------------------------------------- | ------------------------------ |
| 用在方法声明后面，跟的是异常类名             | 用在方法体内，跟的是异常对象名 |
| 表示抛出异常，由该方法的调用者来处理         | 表示抛出异常，由方法体语句处理 |
| 表示出现异常的可能性，并不一定会发生这些异常 | 执行throw一定抛出了某种异常    |

# 记录日志

* Logger构造器

| Modifier(修饰语) |                      构造器                      |                 描述                 |
| ---------------- | :----------------------------------------------: | :----------------------------------: |
| `protected`      | `Logger(String name, String resourceBundleName)` | 构建命名子系统的记录器的受保护方法。 |

* 常用的Logger方法

| Modifier and Type |                        方法                         |                      描述                      |
| :---------------: | :-------------------------------------------------: | :--------------------------------------------: |
|  `static Logger`  |              `getLogger(String name)`               |       查找或创建一个命名子系统的记录器。       |
|  `static Logger`  | `getLogger(String name, String resourceBundleName)` |       查找或创建一个命名子系统的记录器。       |
|      `void`       |             `setLevel(Level newLevel)`              | 设置日志级别，指定该记录器将记录哪些消息级别。 |
|                   |                                                     |                                                |

* 注意：name是Logger的名称，当名称相同时候，同一个名称的Logger只创建一个。

## Logger的级别

* 定义在**java.util.logging.Level**里面。各级别按降序排列如下：
  * SEVERE（最高值）
  * WARNING
  * INFO
  * CONFIG
  * FINE
  * FINER
  * FINEST(最低值)

* logger默认的级别是INFO，比INFO更低的日志将不显示
* Logger的默认级别定义是在jre安装目录的lib下面。
* 级别 OFF，可用来关闭日志记录，使用级别 ALL 启用所有消息的日志记录。

```java
public class LoggerTest {
    public static void main(String[] args) {
        Logger log = Logger.getLogger("JavaLogger");
        log.setLevel(Level.INFO);

        Logger logger1 = Logger.getLogger("JavaLogger1");
        logger1.setLevel(Level.WARNING);

        log.warning("等级为INFO,输出为Warning");
        log.info("等级为INFO，输出为INFO");

        logger1.warning("等级为Warning，输出为Warning");
        logger1.severe("等级为warning，输出为严重（severe）");

    }
}

```

## Logger的Handler

* Handler 对象从 Logger 中获取日志信息，并将这些信息导出。例如，它可将这些信息写入控制台或文件中，也可以将这些信息发送到网络日志服务中，或将其转发到操作系统日志中。
* 可通过执行 setLevel(Level.OFF) 来禁用 Handler，并可通过执行适当级别的 setLevel 来重新启用。
* Handler 类通常使用 LogManager 属性来设置 Handler 的 Filter、Formatter 和 Level 的默认值。

```java
public class LoggerTest1 {
    public static void main(String[] args) throws IOException {
        Logger log = Logger.getLogger("JavaLogger");
        log.setLevel(Level.INFO);

        Logger log2 = Logger.getLogger("JavaLog.blog");

        //控制台控制器
        ConsoleHandler conhl = new ConsoleHandler();
        conhl.setLevel(Level.ALL);
        log.addHandler(conhl);

        //文件控制器
        FileHandler fileHandler = new FileHandler("D:\\虚幻引擎\\22.txt");
        fileHandler.setLevel(Level.INFO);
        log.addHandler(fileHandler);
        log.info("aaa");
        log2.info("bbb");
        log2.fine("fine");
    }
}
```

## Logger的Formatter

* Formatter 为格式化 LogRecords 提供支持。
* 一般来说，每个日志记录 Handler 都有关联的 Formatter。Formatter 接受 LogRecord，并将它转换为一个字符串。
* 有些 formatter（如 XMLFormatter）需要围绕一组格式化记录来包装头部和尾部字符串。可以使用 getHeader 和 getTail 方法来获得这些字符串。
* LogRecord 对象用于在日志框架和单个日志 Handler 之间传递日志请求。
* LogRecord(Level level, String msg) 用给定级别和消息值构造 LogRecord。

