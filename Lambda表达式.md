# Lambda表达式

## Lambda表达式的标准格式

* Lambda表达式的代码分析
  * （）：里面没有内容，可以看成是方法形式参数为空
  * ->：用箭头指向后面要做的事情
  * {}：包含一段代码，我们称之为代码块，可以看成是方法体中的内容

* Lambda的格式
  * （形式参数）-> {代码块}
  * 形式参数：如果有多个参数，参数之间用逗号隔开；如果没有参数，留空即可
  * ->： 由英文中画线和大于符号组成，固定写法，代表指向动作
  * 代码块：是我们具体要做的事情，也就是以前我们写的方法体内容

### Lambda表达式练习（抽象方法无参无返回值）

```java
//定义一个接口
public interface Flyable {
    void fly();
}

```

```java
//接口实现类
public class FlyableImpl implements Flyable {
    @Override
    public void fly() {
        System.out.println("实现接口飞翔功能开启");
    }
}

```

```java
//测试类
public class DemoFly {
    public static void main(String[] args) {

        //实现接口
        Flyable fa = new FlyableImpl();
        fa.fly();

        //匿名内部类
        usefly(new Flyable() {
            @Override
            public void fly() {
                System.out.println("内部类飞翔功能开启");
            }
        });

        //lambda表达式
        usefly(() -> {
            System.out.println("lambda表达式飞翔功能开启");
        });

    }
    private static void usefly(Flyable flyable) {
        flyable.fly();
    }

}
```

### Lambda表达式练习（抽象方法带参无返回值）

```java
//定义一个接口
public interface Eatable {
    void eat(String s);
}

```

```java
//实现类
public class EatableImpl implements Eatable {
//    private String s;

    @Override
    public void eat(String s) {
        System.out.println(s);
    }
}

```

```java
public class DemoEat {
    public static void main(String[] args) {
        Eatable e = new EatableImpl();
        e.eat("实现接口胡吃海塞开启");

        //匿名内部类
        eatAll(new Eatable() {
            @Override
            public void eat(String s) {
                System.out.println("内部类胡吃海塞");
            }
        });

        //Lambda表达式
        eatAll((String s) -> {
            System.out.println("Lambda表达式胡吃海塞");
        });
    }

    private static void eatAll(Eatable e){
        e.eat("胡吃海塞");
    }
}
```

### Lambda表达式练习（抽象方法带参带返回值）

```java
public interface Sleep {

    int sleep(int a ,int b);
}

```

```java
public class SleepImpl implements Sleep {
    @Override
    public int sleep(int a, int b) {
        return a+b;
    }
}

```

```java
public class DemoSleep {

    public static void main(String[] args) {
        Sleep s = new SleepImpl();
        int sleep = s.sleep(1, 2);

        System.out.println(sleep);

        //匿名内部类

        needSleep(new Sleep() {
            @Override
            public int sleep(int a, int b) {
                System.out.println("内部类睡觉");
                return a + b;
            }
        });

        //Lambda表达式
        needSleep((int a, int b) -> {
            return a + b;
        });

    }

    private static void needSleep(Sleep s) {
        int sleep = s.sleep(3, 2);
        System.out.println(sleep);

    }
}
```

## Lambda 表达式的省略模式

* 省略规则
  * 参数类型可以省略，但是有多个参数的情况下，不能只省略一个
  * 如果参数有且仅有一个，那么小括号可以省略
  * 如果代码块的语句只有一条，可以省略大括号和分号，甚至是return

## Lambda表达式注意事项

* 注意事项：
  * 使用Lambda必须要有接口，并且要求接口有且仅有一个抽象方法
  * 必须有上下文环境，才能推导出Lambda对应的接口
    * 根据局部变量的复制得知Lambda对应的接口：Runnable r = () -> System.out.println("Lambda表达式");
    * 根据调用方法的参数得知Lambda对应的接口：new Thread(() -> System.out.println("Lambda表达式");).start();

## Lambda表达式和匿名内部类的区别

* 所需类型不同
  * 匿名内部类：可以是接口、抽象类、具体类
  * Lambda表达式：只能是接口

* 使用限制不同
  * 如果接口中有且仅有一个抽象方法，可以用Lambda表达式，也可以使用匿名内部类
  * 如果接口中多于一个抽象方法，只能使用匿名内部类，而不能使用Lambda表达式

* 实现原理不同
  * 匿名内部类：编译之后，产生一个单独的.class字节码文件
  * Lambda表达式：编译之后，没有一个单独的.class字节码文件，对应的字节码会在运行时动态生成

