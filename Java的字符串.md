# Java的字符串

## 不可变String

* String对象是不可变的
* String类中每一个看起来会修改String值的方法，实际上都是创建了一个全新的String对象，用以包含修改后的字符串内容，最初的String对象丝毫未动

```java
public class Immutable {
    public static String upcase(String s){
        return s.toUpperCase();
    }

    public static void main(String[] args) {
        String q = "wxs";
        System.out.println(q);//wxs
        String qq = upcase(q);
        System.out.println(qq);//WXS
        System.out.println(q);//wxs
    }
    //当把q传给upcase()方法时，实际传递的是一个拷贝，实际上每当把Sting对象作为方法的参数时，都会复制一份引用，而该引用所指的对象其实一直待在单一的物理位置上，从未动过
}

```

## 重载 "+" 与StringBuilder

* String对象的只读特性使其为不可变
* 操作符"+"可以用来连接String

```java
public class Concatenation {
    public static void main(String[] args) {
        String mango = "mango";
        String s = "abc" + mango + "def" + 47;
        System.out.println(s);
        //编译器自动引入了java.lang.StringBuilder类。虽然我们源码中没有用StringBuilder类，但是编译器自作主张使用了，因为更高效
        
        /*
            理论：编译器创建了一个StringBulider对象，用以构造最终的String，并为每个字符串调用一次StringBulider的append()方法，
            总计4此，最后调用toString()生成结果，并存为s
        * */
    }
}

```

> 用于String的 "+" 与 "+=" 是Java中仅有的两个重载过的操作符，而Java并不允许程序员重载任何操作符

## 无意识的递归

* Java中的每个类从根本上都是继承自Object，标准容器类自然也不例外，因此容器类都有toString()方法，并覆写该方法，使得它生成的String结果能表达容器自身，以及容器所包含的对象
* 例：ArrayList.toString()，它会遍历ArrayList中包含的所有对象，调用每个元素上的toString()方法
* 当你希望toString()方法打印出对象的内存地址，如果使用this关键字会出现递归的调用（正确做法应该使用super.toString()方法）

```java
public class InfiniteRecursion {
    public String toString(){
//        return "当前类对象的内存地址：" + this +"\n";//java.lang.StackOverflowError
        return "当前类对象的内存地址：" + super.toString() +"\n";
    }

    public static void main(String[] args) {
        ArrayList<InfiniteRecursion> v = new ArrayList<> ();
        for (int i = 0; i <10 ; i++) {
            v.add(new InfiniteRecursion());
        }
        System.out.println(v);
    }
}

```

## String上的操作

* String对象具备的一些基本方法，重载方法归纳在同一行中

| Modifier and Type |                    方法                    |                         应用（描述）                         |
| :---------------: | :----------------------------------------: | :----------------------------------------------------------: |
|       `int`       |                 `length()`                 |                      String中字符的个数                      |
|      `char`       |                  charAt()                  |                 返回指定索引处的 `char`值。                  |
|     `byte[]`      |                `getBytes()`                |                     复制byte到目标数组中                     |
|     `char[]`      |              `toCharArray()`               |                将此字符串转换为新的字符数组。                |
|     `boolean`     |         `equals(Object anObject)`          |                将此字符串与指定对象进行比较。                |
|       `int`       |     `compareTo(String anotherString)`      |                  按字典顺序比较两个字符串。                  |
|     `boolean`     |         `contains(CharSequence s)`         |      当且仅当此字符串包含指定的char值序列时才返回true。      |
|     `boolean`     |        `startsWith(String prefix)`         |              测试此字符串是否以指定的前缀开头。              |
|     `boolean`     |         `endsWith(String suffix)`          |              测试此字符串是否以指定的后缀结尾。              |
|       `int`       |             `indexOf(int ch)`              |           返回指定字符第一次出现的字符串内的索引。           |
|       `int`       |     `indexOf(int ch,  int fromIndex)`      | 返回指定字符第一次出现的字符串内的索引，以指定的索引开始搜索。 |
|       `int`       |           `indexOf(String str)`            |         返回指定子字符串第一次出现的字符串内的索引。         |
|       `int`       |   `indexOf(String str,  int fromIndex)`    | 返回指定子串的第一次出现的字符串中的索引，从指定的索引开始。 |
|     `String`      |        `substring(int beginIndex)`         |        返回一个字符串，该字符串是此字符串的子字符串。        |
|     `String`      | `substring(int beginIndex,  int endIndex)` |        返回一个字符串，该字符串是此字符串的子字符串。        |
|     `String`      |            `concat(String str)`            |             将指定的字符串连接到该字符串的末尾。             |
|     `String`      |   `replace(char oldChar,  char newChar)`   | 返回从替换所有出现的导致一个字符串 `oldChar` ，在这个字符串  `newChar` 。 |
|     `String`      |              `toUpperCase()`               |  将此 `String`所有字符转换为大写，使用默认语言环境的规则。   |
|     `String`      |              `toLowerCase()`               |   使用默认语言环境的规则将此 `String`所有字符转换为小写。    |
|     `String`      |                  `trim()`                  |  返回一个字符串，其值为此字符串，并删除任何前导和尾随空格。  |
|  `static String`  |                `valueOf()`                 |                 返回一个表示参数内容的String                 |
|     `String`      |                 `intern()`                 |                  返回字符串对象的规范表示。                  |

## 格式化输出

### Formatter类

* 在Java中，所有新的格式化功能都由java.util.Formatter类处理 

```java
public class Turtle {
    private String name;
    private Formatter f;

    public Turtle(String name, Formatter f) {
        this.name = name;
        this.f = f;
    }
    public void move(int x, int y,float z){
        /**
         * %d表示一个整数，%f表示一个浮点数(float或者double),%s表示一个String类型参数
         */
        //Formatter format​(String format, Object... args) 使用指定的格式字符串和参数将格式化的字符串写入此对象的目标。  
        f.format("%s The Turtle is at (%d,%d,%f)\n",name,x,y,z);
    }

    public static void main(String[] args) {
        PrintStream out = System.out;
        Turtle wxs = new Turtle("wxs", new Formatter(System.out));
        Turtle wxs1 = new Turtle("wxs1", new Formatter(out));
        
        wxs.move(1,2,1.111F);
        wxs1.move(2,3,33.33F);
    }
}

```

### 格式化说明符

