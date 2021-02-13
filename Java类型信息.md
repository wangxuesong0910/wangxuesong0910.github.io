# Java类型信息

* 运行时类型信息使得你可以在程序运行时发现和使用类型信息

## RTTI(Run-Time Type Identification)运行时类型识别

![](.\为什么需要RTTI.png)

```java
/**
 * 如上图所示：
 *              Shape接口中动态绑定了draw()方法，目的就是让客户端程序员使用泛化的Shape引用来调用draw()。
 *              draw()在所有派生类里都会被覆盖，并且由于它是动态绑定的，所以即使通过泛化的Shape引用来调用，
 *              也能产生正确行为，这就是多态
 */

abstract class Shape{
    void draw(){
        System.out.println(this + ".draw()");
    }
    //toString()被声明为abstract，以此强制继承者覆写该方法，并可以对无格式的Shape的实例化
    abstract public String toString();
}

/**
 * 创建具体对象，将它向上转型为Shape（忽略对象的具体类型），并在后面程序中使用匿名的Shape引用
 */
class Circle extends Shape{
    @Override
    public String toString() {
        return "Circle";
    }
}

class Square extends Shape{
    @Override
    public String toString() {
        return "Square";
    }
}

class Triangle extends Shape{
    @Override
    public String toString() {
        return "Triangle";
    }
}
public class Shapes {
    public static void main(String[] args) {
        /**
         * RTTI最基本的使用形式：
         *          把Shape对象放入List<Shape>的数组时会向上转型，但是向上转型为Shape的时候也丢失了Shape对象的具体类型，
         *          对于数组而言，他们只是Shape类的对象
         *          当从数组中取出元素时，这种容器——实际上它将所有的事物都当做Object持有——会自动将结果转型回Shape
         *
         * 在下面的例子中，RTTI类型转换并不彻底：Object被转型为Shape，而不是转型为Cricle、Square或者Triangle。
         *                              因为目前我们只知道这个List<Shape>保存的都是Shape
         *                              在编译时，将由容器和Java的泛型系统来强制确保这一点；在运行时，由类型转换操作来确保这一点
         */
        List<Shape> shapeList = Arrays.asList(new Circle(), new Square(), new Triangle());
        for (Shape shape:shapeList){
            shape.draw();
        }
    }
}

```

## Class对象

* 类型信息在运行时是如何表示的？
  * 由Class对象完成，它包含了与类有关的信息(Class对象就是用来创建类的所有的“常规”对象的)
  * Java使用Class对象来执行其RTTI

### 类加载器

* 类是程序的一部分，每个类都有一个Class对象
* 每当编写并且编译了一个新类，就会产生一个Class对象（恰当的说：被保存在一个同名的.class文件中）
* Java虚拟机(JVM)将使用被称为"类加载器"的子系统来生成这个对象

> 类加载器子系统实际上可以包含一条类加载器链，但是只有一个原生类加载器，它是JVM实现的一部分，原生类加载器加载的是所谓的可信类，包括Java API类，他们通常是从本地盘加载的，在这条链中，通常许需要添加额外的类加载器，但是如果你有特殊需求(例如以某种特殊的方式加载类，以支持Web服务器应用，或者在网络中下载类)，那么你有一种方式可以挂接额外的类加载类

1. 所有的类都是在对其第一次使用时，动态加载到JVM中的。
2. 当程序创建第一个对类的静态成员的引用时就会加载这个类，这证明构造器也是类的静态方法，即使在构造器之前并没有使用static关键字
3. 因此，使用new操作符创建类的新对象也会被当做对类的静态成员的引用
4. 类加载器首先检查这个类的Class对象是否已经加载，如果尚未加载，默认的类加载器就会根据类名查找.class文件。在这个类的字节码被加载时，他们会接受验证，以确保其没有被破话，并且不包含不良的Java代码
5. 一旦某个类的Class对象被载入内存，它就被用来创建这个类的所有对象

```java
package ReadingBook.ChapterThirteen;

class Candy{
    static{
        System.out.println("加载 Candy");
    }
}
class Gum{
    static{
        System.out.println("加载 Gum");
    }
}
class Cookie{
    static{
        System.out.println("加载 Cookie");
    }
}

public class SweetShop {
    public static void main(String[] args) {
        System.out.println("main方法中");
        new Candy();
        System.out.println("Candy创建已完成");
        try {
            Class.forName("ReadingBook.ChapterThirteen.Gum");
        } catch (ClassNotFoundException e) {
            System.out.println("没有找到Gum");
        }
        System.out.println("使用Class.forName(Gum)创建");
        new Cookie();
        System.out.println("Cookie创建已完成");

    }
}

```

* 如需要使用类，该类还未被加载到内存中，JVM做了如下准备工作
  1. 加载：由类加载器执行的。查找字节码，并从字节码中创建一个Class对象
  2. 链接。在连接阶段将验证类中的字节码，为静态域分配存储空间，并且如果必需的话，将解析这个类创建的对其他类的所有引用
  3. 初始化：如果该类有超类，则对其初始化，执行静态初始化器和静态初始化块，（构造器隐式地是静态的）

### 初始化的惰性

* 仅使用.class语法来获取对类的引用不会引发初始化
* 但是为了产生Class引用，Class.forName()立即就进行了初始化
* 如果一个static final值是"编译期常量"，那么这个值不需要对类进行初始化就可以被读取
* 如果一个static域不是final的，那么在对它访问时，总是要求在它被读取之前，要先进行链接(为这个域分配存储空间)和初始化(初始化该存储空间)

## 空对象

