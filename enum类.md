# enum类

* Java SE5 添加了enum关键字

> 在创建enum时，编译器会自动添加一些有用的特性，例如：它会创建toSting()方法，以便你可以很方便的显示某个enum实例的名字，编译器还会创建oridinal()方法，用来表示某个特定的enum常量的声明顺序，以及satic values() 方法，用来按照enum常量的声明顺序，产生由这些常量值构成的数组

```java
public enum Spiciness {
    NOT,MILD,MEDIUM,HOT,FLAMING
}
```

```java
public class enumDemo01 {
    public static void main(String[] args) {
        Spiciness howHot = Spiciness.MEDIUM;
        System.out.println(howHot);

        //ordinal() 返回此枚举常数的序数（其枚举声明中的位置，其中初始常数的序数为零）。  
        for (Spiciness s:Spiciness.values()){
            System.out.println(s+"，oridinal（顺序：）"+s.ordinal());
        }
    }
}
```

## switch中使用enum

* 由于switch是要在有限的可能值集合中进行选择，因此它与enum正是绝佳的组合
* 可以将enum作为另外一种创建数据类型的方式i，然后直接将所得到的类型拿来使用，这是**关键**所在

```java
public class Burrito {
    Spiciness degree;

    public Burrito(Spiciness degree) {
        this.degree = degree;
    }

    public Burrito() {
    }
    public void describe(){
        switch (degree){
            case NOT:
                System.out.println("not输出");
                break;
            case MILD://case穿透
            case HOT:
                System.out.println("一点点热");
                break;
            case FLAMING:
            case MEDIUM:
                System.out.println("热啊");
        }
    }
}
```

```java
public class enumDemo03 {
    public static void main(String[] args) {
        Burrito burrito = new Burrito(Spiciness.HOT);
        burrito.describe();
    }
}
```

## 基本enum特性

* enum的values()方法，可以遍历enum实例：返回一个数组，该数组中的元素严格保持其再enum中声明时的顺序，因此可以在循环中使用values()数组
* ordinal()方法返回一个int值：这是每个enum实例在声明时的次序，从0开始可以用==来比较enum实例，编译器会自动为你提供equals()和hashCode()方法。
* Enum类实现了Comparable接口，所以它具有compareTo()方法，同时，它还实现了Serializable接口
* 在enum实例上调用getDeclaringClass()方法，我们就能知道所属的enum类型
* name()方法返回enum实例声明时的名字，使用toString()方法效果相同
* valueOf()是在Enum中定义的static方法，根据给定的名字返回相应的enum实例

## 使用接口组织枚举

* 希望扩展原enum中的元素，或者希望子类将enum中的元素进行分类
* 在一个接口内部，创建实现该接口的枚举类，以此将元素分组，可以达到将枚举元素分类组织的目的

```java
public interface Food {
  enum Appetizer implements Food {
    SALAD, SOUP, SPRING_ROLLS;
  }
  enum MainCourse implements Food {
    LASAGNE, BURRITO, PAD_THAI,
    LENTILS, HUMMOUS, VINDALOO;
  }
  enum Dessert implements Food {
    TIRAMISU, GELATO, BLACK_FOREST_CAKE,
    FRUIT, CREME_CARAMEL;
  }
  enum Coffee implements Food {
    BLACK_COFFEE, DECAF_COFFEE, ESPRESSO,
    LATTE, CAPPUCCINO, TEA, HERB_TEA;
  }
} ///:~

```

## 使用EnumSet代替标志

* Java SE5引入EnumSet，为了通过enum创建一种替代品，替代传统的基于int的"位标志"
* 这种标志可以用来表示某种"开/关"信息，不过，使用这种标志，我们最终操作的只是一些bit
* EnumSet的设计充分考虑到了速度因素，内部就是将一个long值作为比特向量

```java
enum AlarmPoints {
    STAIR1, STAIR2, LOBBY, OFFICE1, OFFICE2, OFFICE3, OFFICE4, BATHROOM, UTILITY, KITCHEN
}

public class EnumSets {
    public static void main(String[] args) {
        EnumSet<AlarmPoints> points =
                EnumSet.noneOf(AlarmPoints.class); // Empty set
        points.add(AlarmPoints.BATHROOM);
        print(points);

        points.addAll(EnumSet.of(AlarmPoints.STAIR1, AlarmPoints.STAIR2, AlarmPoints.KITCHEN));
        print(points);

        points = EnumSet.allOf(AlarmPoints.class);
        points.removeAll(EnumSet.of(AlarmPoints.STAIR1, AlarmPoints.STAIR2, AlarmPoints.KITCHEN));
        print(points);

        points.removeAll(EnumSet.range(AlarmPoints.OFFICE1, AlarmPoints.OFFICE4));
        print(points);

        points = EnumSet.complementOf(points);
        print(points);
    }
} /* Output:
[BATHROOM]
[STAIR1, STAIR2, BATHROOM, KITCHEN]
[LOBBY, OFFICE1, OFFICE2, OFFICE3, OFFICE4, BATHROOM, UTILITY]
[LOBBY, BATHROOM, UTILITY]
[STAIR1, STAIR2, OFFICE1, OFFICE2, OFFICE3, OFFICE4, KITCHEN]
*///:~

```

## EnumMap

* EnumMap是一种特殊的Map，他要求其中的键(Key) 必须来自一个enum，由于enum本身限制，所以EnumMap在内部可以由数组实现
* EnumMap速度很快，所以使用enum实例在EnumMap中进行查找操作
* 只能将enum的实例作为键来调用put()方法，其他与使用Map差不多

> 命令设计模式：需要只有一个单一方法的接口，然后从该接口实现具有各自不同的行为的多个子类

```java
enum AlarmPoints1 {
  STAIR1, STAIR2, LOBBY, OFFICE1, OFFICE2, OFFICE3, OFFICE4, BATHROOM, UTILITY, KITCHEN
}
interface Command { void action(); }

public class EnumMaps {
  public static void main(String[] args) {
    EnumMap<AlarmPoints1,Command> em =
      new EnumMap<AlarmPoints1,Command>(AlarmPoints1.class);
    em.put(AlarmPoints1.KITCHEN, new Command() {
      public void action() { print("Kitchen fire!"); }
    });
    em.put(AlarmPoints1.BATHROOM, new Command() {
      public void action() { print("Bathroom alert!"); }
    });
    for(Map.Entry<AlarmPoints1,Command> e : em.entrySet()) {
      printnb(e.getKey() + ": ");
      e.getValue().action();
    }
    try { // If there's no value for a particular key:
      em.get(AlarmPoints1.UTILITY).action();
    } catch(Exception e) {
      print(e);
    }
  }
} /* Output:
BATHROOM: Bathroom alert!
KITCHEN: Kitchen fire!
java.lang.NullPointerException
*///:~

```

* enum实例定义时的次序决定了其在EnumMap中的顺序
* 与常量相关方法相比，EnumMap有一个优点，EnumMap允许程序员改变值对象，而常量相关的方法在编译器就固定了
* 可以用EnumMap实现多路分发