---
title: "JAVA的线程"
date: 2020-07-23T14:30:16+08:00
draft: true
---

# 多线程

## 实现多线程

### 进程

> 进程：正在运行的程序

* 是系统进行资源分配和调用的独立单位
* 每一个进程都有它自己独立的内存空间和系统资源

### 线程

> 线程：是进程中的单个顺序控制流，是一条执行路径

* 单线程：一个进程如果只有一条执行路径，则成为单线程程序
* 多线程：一个进行如果有多条执行路径，则成为多线程程序

### 多线程的实现方式

* 方式一：
  * 定义一个类MyThread继承Thread类
  * 在MyThread类中重写run() 方法
  * 创建MyThread类的对象
  * 启动线程（start()方法启动）

* 两个小问题
  * 为什么要重写run()方法
    * 因为run()是用来封装被线程执行的代码
  * run() 方法和 start() 方法区别？
    * run() ：封装线程执行的代码，直接调用，相当于普通方法的调用
    * start()：启动线程；然后由JVM调用此线程的run() 方法

### 设置和获取线程名称

* Thread类中设置和获取线程名称的方法
  * void setName(String name)：将此线程的名称更改为等于参数name
  * String getName()：返回此线程的名称
  * 通过构造方法也可以设置线程名称

```java
//static thread currentThread() 返回当前正在执行的线程对象的引用
//可以获取当前正在执行的线程对象是谁
Thread.currentThread().getName();
```

### 线程的调度

* 线程有两种调度模型
  * 分时调度模型：所有线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间片
  * 抢占式调度模型：优先让优先级高的线程使用CPU，如果线程的优先级相同，那么会随机选择一个，优先级高的线程获取的CPU时间片相对多一些

> Java使用的是抢占式调度模型
>
> 假如计算机只有一个CPU，那么CPU在某一个时刻只能执行一条指令，线程只有得到CPU时间片，也就是使用权才可以执行指令，所以说多线程程序的执行是有随机性，因为谁抢到CPU的使用权是不一定的

* Thread类中设置和获取线程优先级的方法
  * public final int getPriority()：返回此线程的优先级
  * public final void setPriority(int newPriority)：更改此线程的优先级

> 线程默认优先级是5；线程优先级范围是：1-10
>
> 线程优先级高仅仅表示线程获取的CPU时间片的几率高，但是要在次数比较多，或者多次运行的时候才能看到你想要的结果

### 线程控制

| 方法名                         | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| static void sleep(long millis) | 使当前正在执行的线程停留（暂停执行）指定的毫秒数             |
| void join()                    | 等待这个线程死亡                                             |
| void setDaemon(boolean on)     | 将此线程标记为守护线程，当运行的线程都是守护线程时，java虚拟机将推出 |

### 线程的生命周期

![](http://47.105.182.219/线程生命周期.png)

### 多线程的实现方式二

* 方式二：实现Runnable接口
  * 定义一个类MyRunnable实现Runnable接口
  * 在MyRunnable类中重写run() 方法
  * 创建MyRunnable类的对象
  * 创建Thread类的对象，把MyRunnable对象作为构造方法的参数
  * 启动线程

* 线程的实现方案有两种
  * 继承Thread类
  * 实现Runnable接口

* 相比继承Thread类，实现Runnable接口的好处
  * 避免了Java单继承的局限性
  * 适合多个相同程序的代码去处理同一个资源的情况，把线程和程序的代码、数据有效分离、较好的体现了面向对象的设计思想

### 关于卖票案例的两种实现以及思考

```java
//使用继承Thread方式
public class SellTicket extends Thread{
    private static int tickets = 100;//共享数据tickets
    
     @Override
    public void run() {
        while (true) {
          
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                        System.out.println(Thread.currentThread().getName() + "卖出一张票:");
                        tickets--;
                        System.out.println("当前余票" + (tickets));

                } else {
                    System.out.println("售罄");
                    break;
                }
            
        }
    }
    
}
```

```java
public class Demo18 {
    public static void main(String[] args) {
//        SellTicket st = new SellTicket();
//
//        Thread t1= new Thread(st, "窗口1");
//        Thread t2= new Thread(st, "窗口2");
//        Thread t3= new Thread(st, "窗口3");
        SellTicket t1 = new SellTicket();
        SellTicket t2 = new SellTicket();
        SellTicket t3 = new SellTicket();
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

/*

  由此查看运行结果可看出，当余票为0时，另外两个窗口仍然卖出了两张票，这就牵扯到线程的安全问题了
   具体原理是：当线程1进入if判断当前余票大于0时，之后便进入了阻塞状态，这时线程2进入当前判断（获得了CPU的执行权）以此类推，线程3也是如此，三个线程都做了tickets--操作，这样当剩下一张票时，三个线程都判断还有余票，都进行了tickets--操作，这样便出现了线程安全的问题  
*/
```

***

```java
//实现Runnable接口的方式创建线程
public class MyThread implements Runnable {
    private int ticket = 100;
    Object obj = new Object();

    public MyThread() {
    }

    @Override
    public void run() {
        while (true) {
            
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + "卖票：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            

        }
    }
}
```

```java
public class Demo01 {
    public static void main(String[] args) {
        MyThread m1 = new MyThread();
        Thread thread1 = new Thread(m1,"窗口1");//使用Thread的带参构造方法
        Thread thread2 = new Thread(m1,"窗口2");
        Thread thread3 = new Thread(m1,"窗口3");

        thread1.start();
        thread2.start();
        thread3.start();


    }
}
/*
  道理与继承Thread类相同，都会出现线程安全问题，那么如何解决？

*/
```

### 同步代码块解决线程安全问题

> 锁多条语句操作的共享数据，可以使用同步代码块实现

```java
//格式如下：
synchronized(任意对象){//这个任意对象即为锁，注意：锁必须是唯一的
   多条语句操作共享数据的代码
}
```

* synchronized(任意对象)：就相当于给代码加锁了，任意对象就可以看成是一把锁

> synchronized的功能是：首先判断对象或方法的***互斥锁***是否存在，若存在就获得互斥锁，然后就可以执行紧随其后的临界代码段或方法体，如果对象或方法的互斥锁不在（已经被其他线程拿走，就进入等待状态），直到获得互斥锁
>
> 注意：当被synchronized限定的代码段执行完，就会自动释放互斥锁

* 同步的好处和弊端：
  * 好处：解决了多线程的数据安全问题
  * 弊端：当线程很多时，因为每个线程都会区判断同步上的锁，很耗费资源，无形中降低程序的运行效率

```java
//实际操作以及分析如下
//使用继承Thread方式
public class SellTicket extends Thread{
    private static int tickets = 100;//共享数据tickets
    private static Object obj = new Object();//使用Object对象充当锁
     @Override
    public void run() {
        while (true) {
            
  //这个锁可以是任意对象，可以是上面的obj，也可以是this指代本类对象，也可以SellTicket.class(反射)
          synchronized(obj){//这里我使用obj或者this，那么在下面的main方法中会出现一个小问题
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                        System.out.println(Thread.currentThread().getName() + "卖出一张票:");
                        tickets--;
                        System.out.println("当前余票" + (tickets));

                } else {
                    System.out.println("售罄");
                    break;
                }
           }
        }
    }
    
}
```

```java
public class Demo18 {
    public static void main(String[] args) {
//        SellTicket st = new SellTicket();
//
//        Thread t1= new Thread(st, "窗口1");
//        Thread t2= new Thread(st, "窗口2");
//        Thread t3= new Thread(st, "窗口3");
        
        //此处当我们的Object对象不声明成static的时候，这里仍然会有线程安全问题，为什么？
        //因为下面的代码实例化了三个SellTicket类的对象，每个类的对象都创建了一个Object对象，这样锁就变成了三把锁，锁变得不唯一了起来，同理用this充当锁也会出现线程安全问题，上面注释掉的代码可以解决当Object对象不声明成static时，出现的线程安全问题，但是我们可以使用SellTicket.class（唯一）来避免这样锁不唯一的情况
        SellTicket t1 = new SellTicket();
        SellTicket t2 = new SellTicket();
        SellTicket t3 = new SellTicket();
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

//使用实现Runnable接口的方式不会出现此问题
```

***

```java
//使用实现Runnable接口的方式创建线程并解决线程安全问题
public class MyThread implements Runnable {
    private int ticket = 100;
    Object obj = new Object();

    public MyThread() {
    }

    @Override
    public void run() {
        while (true) {
            synchronized (this) {//由于实例化使用的Thread带参构造方法，对象m1是唯一的，这里使用this指代本类对象也不会出现线程安全问题，因为它是唯一的
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + "卖票：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }

        }
    }
}
```

```java
public class Demo01 {
    public static void main(String[] args) {
        MyThread m1 = new MyThread();
        Thread thread1 = new Thread(m1,"窗口1");//使用Thread的带参构造方法
        Thread thread2 = new Thread(m1,"窗口2");
        Thread thread3 = new Thread(m1,"窗口3");

        thread1.start();
        thread2.start();
        thread3.start();


    }
}
```

### 同步方法

> 同步方法: 就是把synchronized关键字加到方法上

* 格式：
  * 修饰符 synchronized 返回值类型 方法名（方法参数）{}

* 同步方法的锁对象是什么呢？
  * this

> 同步静态方法：就是把synchronized关键字加到静态方法上

* 格式：
  * 修饰符 static synchronized 返回值类型  方法名（方法参数）{}

* 同步静态方法的锁对象是什么呢？
  * 类名.class

```java
//同步方法使用详解
public class MyThread implements Runnable {
    private static int ticket = 100;
    Object obj = new Object();

    public MyThread() {
    }

    @Override
    public void run() {
        while (true) {
            if (ticket % 2 == 0) {
//                synchronized (obj) {//当锁为obj时，发现仍然会出现线程不安全问题，这说明同步方法的锁应该是本类对象
                synchronized (this) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (ticket > 0) {
                        System.out.println(Thread.currentThread().getName() + "卖票：" + ticket);
                        ticket--;
                    }

                }
            }else{
                sellTicket();
            }
        }
    }

    private synchronized void sellTicket() {
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        if (ticket > 0) {
            System.out.println(Thread.currentThread().getName() + "卖票：" + ticket);
            ticket--;
        }
    }
}

```

```java
//同步方法测试
public class Demo01 {
    public static void main(String[] args) {
        MyThread m1 = new MyThread();
        Thread thread1 = new Thread(m1,"窗口1");
        Thread thread2 = new Thread(m1,"窗口2");
        Thread thread3 = new Thread(m1,"窗口3");

        thread1.start();
        thread2.start();
        thread3.start();

    }
}
```

***

```java
//同步静态方法
public class MyThread implements Runnable {
    private static int ticket = 100;//共享数据（临界资源）使用同步静态方法时应声明数据为静态
    Object obj = new Object();

    public MyThread() {
    }

    @Override
    public void run() {
        while (true) {
            if (ticket % 2 == 0) {
//                synchronized (obj) {
//                synchronized (this) {//使用this关键字作为锁，当方法使用同步静态方法时，会出现线程安全问题（重票错票），查看源码发现同步静态方法的锁为: 类名.class
                synchronized (MyThread.class) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (ticket > 0) {
                        System.out.println(Thread.currentThread().getName() + "卖票：" + ticket);
                        ticket--;
                    }

                }
            }else{
                sellTicket();
            }
        }
    }

    private static synchronized void sellTicket() {
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        if (ticket > 0) {
            System.out.println(Thread.currentThread().getName() + "卖票：" + ticket);
            ticket--;
        }
    }
}
```

### 线程安全的类

* StringBuffer
  * 线程安全，可变的字符序列
  * 从版本JDK5开始，被StringBuilder替代，通常应该使用StringBuilder类，因为它支持所有相同的操作，但它更快，因为它不执行同步

* Vector
  * 从java2平台v1.2开始，该类改进了List接口，使其成为Java Collections Framework的成员，与新的集合实现不同，Vector被同步，如果不需要线程安全的实现，建议使用ArrayList代替Vector

* Hashtable
  * 该类实现了一个哈希表，他将键映射到值，任何非null对象都可以作键或者值
  * 从java2平台v1.2开始，该类进行了改进，实现了Map接口，使其成为Java Collections Framework的成员。与新的集合实现不同，Hashtable被同步，如果不需要线程安全的实现，建议使用HashMap代替Hashtable

## Lock锁

> 虽然我们可以理解同步代码块和同步方法的锁对象问题，但是我们没有直接看到在哪里加上了锁，在哪里释放了锁，为了更清晰的表达如何加锁和释放锁，JDK5以后提供了一个新的锁对象Lock

> Lock实现提供比使用synchronized方法和语句可以获得更加广泛的锁定操作

* Lock中提供了获得锁和释放锁的方法
  * void lock()：获得锁
  * void unlock()：释放锁

* Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来实例化
* ReentrantLock的构造方法
  * ReentrantLock（）：创建一个ReentrantLock的实例

```java
//使用Lock锁保证线程安全
public class MyThread implements Runnable {
    private static int ticket = 100;
   // Object obj = new Object();
    private Lock lock = new ReentrantLock();


    public MyThread() {
    }

    @Override
    public void run() {
        while (true) {
            try {//一般使用try{}catch(){}finally{}保证释放锁
                lock.lock();
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + "卖票：" + ticket);
                    ticket--;
                }
            }finally {
                lock.unlock();
            }


        }
    }
}
```

### Lock锁与synchronized对比

1. Lock是显式锁（手动开启和关闭锁，牢记要关闭锁），synchronized是隐式锁，出了作用域自动释放
2. Lock只有代码块锁，synchronized有代码块锁和方法锁
3. 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好，并且具有更好的扩展性（提供更多的子类）
4. 优先使用：Lock—>同步代码块（已经进入了方法体，分配了相应的资源）—>同步方法（在方法体之外）

## 线程的死锁

* 为什么会出现死锁？
  * 不同的线程分别占用对方需要的同步资源不放弃，都在等对方放弃自己需要的同步资源，就形成了线程的死锁
  * 出现死锁后不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续

## 设计模式：单例模式的懒汉式线程安全问题

> 参考：JAVA的单例模式

## 生产者和消费者模式概述

* 为了体现生产和消费过程中的等待和唤醒，Java就提供了几个方法供我们使用，这几个方法在Object类中Object类的等待和唤醒方法：

| 方法名           | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| void wait()      | 导致当前线程等待，直到另一个线程调用该对象的notify() 方法或notifyAll() 方法 |
| void notify()    | 唤醒正在等待对象监视器的单个线程（对象监视器：锁）           |
| void notifyAll() | 唤醒正在等待对象监视器的所有线程                             |

```java
//生产者消费者案例演示
//定义奶箱类
public class Box {
    private int milk;
    private boolean state = false;

    public synchronized void putMilk(int milk) {
        if (state) {
            try {
                wait();//sleep()方法和wait()方法一样，都能使线程阻塞，但这两个方法是有区别的：wait()方法在放弃CPU资源的同时交出了资源的控制权，而sleep()方法则无法做到这一点
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        this.milk = milk;
        System.out.println("生产者生产了第" + milk + "瓶奶");
        state = true;
        notifyAll();

    }

    public synchronized void getMilk() {
        if (!state) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("消费者取走了第" + milk + "瓶奶");
        state = false;
        notifyAll();

    }


}
```

```java
public class Producer implements Runnable {
    private Box box;

    public Producer(Box box) {
        this.box = box;
    }

    @Override
    public void run() {

        while (true){

            box.getMilk();
        }
    }
}

```

```java
public class Customer implements Runnable {
    private Box box;

    public Customer(Box box) {
        this.box = box;
    }

    @Override
    public void run() {
        for (int i = 1; i < 30; i++) {
            box.putMilk(i);
        }

    }
}

```

```java
public class MilkDemo {
    public static void main(String[] args) {
        Box box = new Box();
        Producer producer = new Producer(box);
        Customer customer = new Customer(box);

        Thread t1 = new Thread(producer);
        Thread t2 = new Thread(customer);
        t1.start();
        t2.start();
    }
}

```

##  进一步说明synchronized

1. synchronized锁定的通常是临界代码，由于所有锁定同一个临界代码的线程之间，在synchronized代码块上是互斥的，也就是说，这些线程的synchronized代码块之间是串行执行的，不再是相互交替穿插并发执行，因而保证了synchronized代码块操作的原子性
2. synchronized代码块中的代码数量越少越好，包含的范围越小越好，否则就会失去多线程并发执行的很多优势
3. 若两个或多个线程锁定的不是同一个对象，则它们的synchronized代码块可以互相交替穿插并发执行
4. 所有的非synchronized代码块或方法都可以自由调用，如线程A获得了对象的互斥锁，调用对象的synchronized代码块，其他线程仍然可以自由调用该对象的所有非synchronized方法和代码
5. 若线程A获得了对象Object1的互斥锁，调用对象Object1的synchronized代码块或方法，则线程A仍然可以调用对象Object1的其他synchronized代码块或方法，这是因为线程A已经获得了对象Object1的互斥锁了，线程A同时可以调用另一个对象Object2的synchronized代码块或方法，这意味着线程A同时拥有对象Object1和对象Object2的互斥锁
6. 在任何时刻，一个对象的互斥锁只能被一个线程锁拥有
7. 之有当一个线程执行完它所调用的对象的所有synchronized代码块或方法时，该线程才会释放这个对象的互斥锁
8. 临界代码中的共享变量应定义为private性，否则其他类的方法可能直接访问和操作该共享变量，这样synchronized的保护就失去了意义
9. 由于（8）的原因，只能用临界代码中的方法访问共享变量，故锁定的对象通常是this，即通常格式都是：synchronized（this）{...}
10. 一定要保证，所有对临界代码中共享变量的访问与操作均在synchronized代码块中进行
11. 通常共享变量都是私有静态变量
12. 对于一个static型的方法，即类方法，要么整个方法是synchronized，要么整个方法不是synchronized
13. 如果synchronized用在类声明中，则表示该类中的所有方法都是synchronized的

# 小结

1. 线程（thread）是指程序的运行流程。“多线程”的机制可以同时运行好几个程序块，使程序运行的效率变得更高，也可以克服传统程序语言无法解决的问题
2. 多任务与多线程是两个不同的概念，多任务是针对操作系统而言的，表示操作系统可以同时运行多个应用程序；而多线程是指有一个程序而言的，表示在一个程序内部可以同时执行多个线程
3. 创建线程有两种方法（实际可以是四种）：一种是继承Thread类；另一种是用户在定义自己的类中实现Runnable接口
4. run（）方法给出了线程要执行的任务。若是派生自Thread类，必须把线程的程序代码编写在run（）方法内，实现覆盖操作；若是实现Runnable接口，必须在实现Runnable接口的类里定义run（）方法
5. 如果在类中要激活线程，必须先准备好下列两件事情：
   1. 此类必须是派生子Thread类或实现Runnable接口，使自己成为它的子类
   2. 线程的处理必须卸载run（）方法内
      

6. 每一个线程，在其创建和消亡之前，均会处于下列五种状态之一：新建状态、就绪状态、运行状态、阻塞状态和消亡状态
7. 阻塞状态的线程一般情况下可由下列的情况所产生：
   - [ ] 该线程调用对象的wait（）方法
   - [ ] 该线程本身调用了sleep（）方法
   - [ ] 该线程和另一个线程join（）在一起
   - [ ] 有优先级更高的线程处于就绪状态

8. 解除阻塞的原因有：
   - [ ] 如果线程使由调用对象的wait方法所阻塞的，则该对象的notify（）方法被调用时可解除阻塞；
   - [ ] 线程进入睡眠（sleep）状态，但指定的睡眠时间到了

9. 当线程的run（）方法运行结束，则线程进入消亡状态
10. Thread类中的sleep（）方法可以用来控制线程睡眠时间，睡眠时间的长短全看sleep（）方法里的参数而定，单位为千分之一秒
11. 要简单地设计线程的有序执行，可调用join（）方法。join（）方法会抛出InterruptedException异常，所以编程时必须把jioin（）方法编写在try-catch块内
12. 线程在运行时，因不需要外部的数据或方法，就不必关系其他线程的状态或行为，这样的线程成为独立、不同步的或是异步执行的
13. 被多个线程对共享的数据在同一时刻只允许一个线程处于操作之中，这就是同步控制中的线程间互斥
14. 当一个线程对共享数据进行操作时，在没有完成相关操作之前，应使之成为一个“原子操作”，即不允许其他线程打断它，否则可能会破坏数据的完整性，而得到错误的处理结果
15. synchronized锁定的是一个具体对象，通常是临界区对象。所有锁定同一个对象的线程之间，在synchronized代码块上是互斥的，也就是说，这些线程的synchronized块之间是串行执行的，不再是互相交替穿插并发执行，因而保证了synchronized代码块操作的原子性
16. 由于所有锁定同一个对象的线程之间，在synchronized代码块上是互斥的，这些线程的synchronized代码块之间是串行执行的，故synchronized代码块中的代码数量越少越好，包含的范围越小越好，否则多线程就会失去很多并发执行的优势
17. 任何时刻，一个对象的互斥锁只能被一个线程所拥有
18. 只有当一个线程执行完它所调用对象的所有synchronized代码块或方法时，该线程才会自动释放这个对象的互斥锁
19. 一定要保证所有对临界区共享变量的访问与操作均在synchronized代码块中进行