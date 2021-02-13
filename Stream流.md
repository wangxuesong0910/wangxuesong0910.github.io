# Stream流

## Stream流的生成方式

1. 生成流（实例化）
   * 通过数据源（集合，数组等）生成流
   * list.stream()

2. 中间操作
   * 一个流后面可以跟随零个或多个中间操作，其目的主要是打开流，做出某种程度的数据***过滤/映射***，然后返回一个新的流交给下一个操作使用
   * filter()

3. 终结操作
   * 一个流只能有一个终结操作，当这个操作执行后，流就被使用“光”了，无法再被操作，所以这必定是流的最后一个结果
   * forEach()

4. Stream流的常见生成方式
   * Collection体系的集合可以使用默认方法stream()生成流
     
     * default Stream<E> stream()
     
   * Map体系的集合间接的生成流
   
   * 数组可以通过Stream接口的静态方法of（T...values）生成流
   
   * 创建无限流(通常用来造数据)
   
     * ```java
       //迭代
       static <T> Stream<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator<T> next) 
       ```
   
   * 生成
   
     * ```java
       //返回无限顺序无序流，其中每个元素由提供的 Supplier生成。 
       static <T> Stream<T> generate(Supplier<? extends T> s) 
       ```
   
5. Stream和Collection集合的区别：

   * Collection是一种静态的内存数据结构，主要面向内存，存储在内存中
   * Stream是有关计算的，主要面向CPU，通过CPU实现计算

```java
public class StreamDemo01 {
    public static void main(String[] args) {
        //Collection体系的集合可以使用默认方法stream()生成流

        List<String> list = new ArrayList<>();
        Stream<String> listStream = list.stream();

        Set<String> set = new HashSet<>();
        Stream<String> setStream = set.stream();

        //Map体系的集合间接的生成流

        Map<String ,Integer> map = new HashMap<>();

        Stream<String> mapStream = map.keySet().stream();
        Stream<Integer> valueStream = map.values().stream();
        Stream<Map.Entry<String, Integer>> meStream = map.entrySet().stream();

        //数组可以通过Stream接口的静态方法of（T...values）生成流
        String[] ss = {"aa","bb","ss"};

        Stream<String> s1 = Stream.of(ss);

        //创建无线流
        //迭代：static <T> Stream<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator<T> next)
        //static <T> Stream<T> iterate​(T seed, UnaryOperator<T> f)
        //seed - 初始元素
        //hasNext - 应用于元素以确定流何时终止的谓词。
        //next - 要应用于前一个元素以生成新元素的函数
        //f -  要应用于前一个元素以生成新元素的函数
        Stream.iterate(0,integer -> integer+2).limit(10).forEach(System.out::println);


        //生成
        //返回无限顺序无序流，其中每个元素由提供的 Supplier生成。
        //static <T> Stream<T> generate(Supplier<? extends T> s)
        // s - Supplier的生成元素
        Stream.generate(Math::random).limit(10).forEach(System.out::println);
    }

}


```

## Stream流的常见中间操作方法

* Stream<T> filter(Predicate predicate)：用于对流中的数据进行过滤
  * Predicate接口中的方法：boolean test(T t)：对给定的参数进行判断，返回一个布尔值

* Stream<T> limit(long maxSize)：返回此流中的元素组成的流，截取前指定参数个数的数据
* Stream<T> skip(long n)： 跳过指定参数个数的数据，返回由该流的剩余元素组成的流

```java
public class StreamDemo02 {

    public static void main(String[] args) {
        ArrayList<String> arr = new ArrayList<>();

        arr.add("aaa");
        arr.add("abb");
        arr.add("ab");
        arr.add("aa");
        arr.add("bbb");
        arr.add("ccc");

        //筛选以"a"开头的元素
        Stream<String> stream = arr.stream().filter(s -> s.startsWith("a"));
        //筛选以"a"开头的元素且长度为3
        Stream<String> stream1 = arr.stream().filter(s -> s.startsWith("a")).filter(s -> s.length()==3);

        stream.forEach(System.out::println);

        System.out.println("-------------");

        stream1.forEach(System.out::println);


        System.out.println("------------------");

        //取出前3个元素
        Stream<String> limit = arr.stream().limit(3);
        limit.forEach(System.out::println);

        System.out.println("------------------");

        //跳过3个元素，把剩下的在控制台输出
        Stream<String> skip = arr.stream().skip(3);
        skip.forEach(System.out::println);


        System.out.println("------------------");


        //跳过2个元素，把剩下的前两个元素在控制台输出
        Stream<String> limit1 = arr.stream().skip(2).limit(2);
        limit1.forEach(System.out::println);
    }
}

```

* static<T> Stream<T> concat(Stream a,Stream b)：合并a和b两个流为一个流
* Stream<T> distinct()：返回由该流的不同元素（根据Object.equals(Object)）组成的流

```java
public class StreamDemo05 {
    public static void main(String[] args) {
        ArrayList<String> arr = new ArrayList<>();

        arr.add("aaa");
        arr.add("abb");
        arr.add("ab");
        arr.add("ab");
        arr.add("aa");
        arr.add("aa");
        arr.add("bbb");
        arr.add("ccc");


        //去除前4个数据组成一个流
        Stream<String> limit = arr.stream().limit(4);

        //跳过2个数据组成一个流
        Stream<String> skip = arr.stream().skip(2);

        //合并前两个得到的流，并把结果输出
//        Stream.concat(limit, skip).forEach(System.out::println);
        
        //合并前两个流，把结果输出控制台，要求字符串元素不能重复
       Stream.concat(limit,skip).distinct().forEach(System.out::println);



    }
}

```

* Stream<T> sorted()：返回此流的元素组成的流，根据自然元素排序 
* Stream<T> sorted(Comparator conmapator)：返回由该流的元素组成的流，根据提供的Comparator进行排序

```java
public class StreamDemo06 {
    public static void main(String[] args) {
        ArrayList<String> arr = new ArrayList<>();

        arr.add("aaa");
        arr.add("ab");
        arr.add("ab");
        arr.add("aa");
        arr.add("bbb");
        arr.add("abb");
        arr.add("aa");
        arr.add("ccc");

        //按照字母顺序把数据输出在控制台
//        arr.stream().sorted().forEach(System.out::println);

        //按照字符串长度把数据在控制台输出

        arr.stream().sorted(new Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                int i = s1.length() - s2.length();
                int j = i == 0 ? s1.compareTo(s2) : i;
                return j;
            }
        }).forEach(System.out::println);

        


    }
}
```

* <R> Stream<R> map(Function mapper)：返回由给定函数应用于此流的元素的结果组成的流
  * Function接口中的方法          R  apply(T  t)

* IntStream mapToInt(ToIntFunction mapper)：返回一个IntStream其中包含将给定函数应用于此流的元素的结果
  * IntStream：表示原始int 流
  * ToIntFunction接口中的方法   ：int applyAsInt（T Value）

```java
public class StreamDemo07 {
    public static void main(String[] args) {
        ArrayList<String> arr = new ArrayList<>();

        arr.add("11");
        arr.add("22");
        arr.add("33");
        arr.add("44");

        //将集合中的字符串数据转换为整数之后在控制台输出
//        arr.stream().map(s->Integer.parseInt(s)).forEach(System.out::println);
        arr.stream().map(Integer::parseInt).forEach(System.out::println);
        
        arr.stream().mapToInt(Integer::parseInt).forEach(System.out::println);
        
        //int sum()：返回 此流中元素的总和
        int sum = arr.stream().mapToInt(Integer::parseInt).sum();
        System.out.println(sum);
    }
}

```

## Stream流的常见终结操作方法

* void forEach(Consumer action)：对此流的每个元素执行操作
  * Consumer接口中的方法      void accept(T t)：对给定的参数执行此操作

* long count()：返回此流中的元素数

```java
public class StreamDemo08 {
    public static void main(String[] args) {
        ArrayList<String> arr = new ArrayList<>();

        arr.add("aaa");
        arr.add("ab");
        arr.add("ab");
        arr.add("aa");
        arr.add("bbb");
        arr.add("abb");
        arr.add("aa");
        arr.add("ccc");

        //把集合中的元素在控制台输出
        arr.stream().forEach(System.out::println);
        //统计以"a"开头的元素，把统计结果在控制台输出
        long count = arr.stream().filter(s -> s.startsWith("a")).count();
        System.out.println(count);
    }
}

```

```java
public class StreamAPITest2 {

    //1-匹配与查找
    @Test
    public void test1(){
        List<Employee> employees = EmployeeData.getEmployees();

//        allMatch(Predicate p)——检查是否匹配所有元素。
//          练习：是否所有的员工的年龄都大于18
        boolean allMatch = employees.stream().allMatch(e -> e.getAge() > 18);
        System.out.println(allMatch);

//        anyMatch(Predicate p)——检查是否至少匹配一个元素。
//         练习：是否存在员工的工资大于 10000
        boolean anyMatch = employees.stream().anyMatch(e -> e.getSalary() > 10000);
        System.out.println(anyMatch);

//        noneMatch(Predicate p)——检查是否没有匹配的元素。
//          练习：是否存在员工姓“雷”
        boolean noneMatch = employees.stream().noneMatch(e -> e.getName().startsWith("雷"));
        System.out.println(noneMatch);
//        findFirst——返回第一个元素
        Optional<Employee> employee = employees.stream().findFirst();
        System.out.println(employee);
//        findAny——返回当前流中的任意元素
        Optional<Employee> employee1 = employees.parallelStream().findAny();
        System.out.println(employee1);

    }

    @Test
    public void test2(){
        List<Employee> employees = EmployeeData.getEmployees();
        // count——返回流中元素的总个数
        long count = employees.stream().filter(e -> e.getSalary() > 5000).count();
        System.out.println(count);
//        max(Comparator c)——返回流中最大值
//        练习：返回最高的工资：
        Stream<Double> salaryStream = employees.stream().map(e -> e.getSalary());
        Optional<Double> maxSalary = salaryStream.max(Double::compare);
        System.out.println(maxSalary);
//        min(Comparator c)——返回流中最小值
//        练习：返回最低工资的员工
        Optional<Employee> employee = employees.stream().min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
        System.out.println(employee);
        System.out.println();
//        forEach(Consumer c)——内部迭代
        employees.stream().forEach(System.out::println);

        //使用集合的遍历操作
        employees.forEach(System.out::println);
    }

    //2-归约
    @Test
    public void test3(){
//        reduce(T identity, BinaryOperator)——可以将流中元素反复结合起来，得到一个值。返回 T
//        练习1：计算1-10的自然数的和
        List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
        Integer sum = list.stream().reduce(0, Integer::sum);
        System.out.println(sum);


//        reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。返回 Optional<T>
//        练习2：计算公司所有员工工资的总和
        List<Employee> employees = EmployeeData.getEmployees();
        Stream<Double> salaryStream = employees.stream().map(Employee::getSalary);
//        Optional<Double> sumMoney = salaryStream.reduce(Double::sum);
        Optional<Double> sumMoney = salaryStream.reduce((d1,d2) -> d1 + d2);
        System.out.println(sumMoney.get());

    }

    //3-收集
    @Test
    public void test4(){
//        collect(Collector c)——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法
//        练习1：查找工资大于6000的员工，结果返回为一个List或Set

        List<Employee> employees = EmployeeData.getEmployees();
        List<Employee> employeeList = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toList());

        employeeList.forEach(System.out::println);
        System.out.println();
        Set<Employee> employeeSet = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toSet());

        employeeSet.forEach(System.out::println);




    }
}
```

### 规约

```java
public class StreamDemo08 {
    public static void main(String[] args) {
        //Optional<T> reduce​(BinaryOperator<T> accumulator)
        // 使用 associative累积函数对此流的元素执行 reduction ，并返回描述减小值（如果有）的 Optional 。
        //        练习1：计算1-10的自然数的和
        List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
        Integer sum = list.stream().reduce(0, Integer::sum);
        System.out.println(sum);


//        reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。返回 Optional<T>
//        练习2：计算公司所有员工工资的总和
        List<Employee> employees = EmployeeData.getEmployees();
        Stream<Double> salaryStream = employees.stream().map(Employee::getSalary);
//        Optional<Double> sumMoney = salaryStream.reduce(Double::sum);
        Optional<Double> sumMoney = salaryStream.reduce((d1, d2) -> d1 + d2);
        System.out.println(sumMoney.get());
    }
}

```

### 收集



## 练习一

```java
public class StreamDemo09 {
    /**
     * 现在有两个ArrayList集合，分别存储6个男演员名称和6个女演员名称，要求完成如下操作
     * 男演员只要名字为3个字的前三个人
     * 女演员只要姓为“林”的，并且不要第一个人
     * 把过滤后的男演员姓名和女演员姓名合并到一起
     *
     * 把上一步操作后的元素作为构造方法的参数创建演员对象，遍历数据
     * 演员类Actor已经提供，里面有一个成员变量，一个带参构造方法，以及成员变量对应的get/set方法
     */

    public static void main(String[] args) {
        //创建集合
        ArrayList<String> manList = new ArrayList<>();

        manList.add("周润发");
        manList.add("成龙");
        manList.add("刘德华");
        manList.add("吴京");
        manList.add("周星驰");
        manList.add("李连杰");

        ArrayList<String> womanList = new ArrayList<>();

        womanList.add("林心如");
        womanList.add("张曼玉");
        womanList.add("林青霞");
        womanList.add("柳岩");
        womanList.add("林志玲");
        womanList.add("王祖贤");

        //男演员只要名字为3个字的前三个人
        Stream<String> mfl = manList.stream().filter(s -> s.length() == 3).limit(3);

        //女演员只要姓为“林”的，并且不要第一个人
        Stream<String> wfs = womanList.stream().filter(s -> s.startsWith("林")).skip(1);

        //把过滤后的男演员姓名和女演员姓名合并到一起
        Stream<String> concat = Stream.concat(mfl, wfs);

//        把上一步操作后的元素作为构造方法的参数创建演员对象，遍历数据
//        concat.map(s -> new Actor(s)).forEach(s->System.out.println(s.getName()));
        concat.map(Actor::new).forEach(s->System.out.println(s.getName()));
    }
}

public class Actor {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Actor(String name) {
        this.name = name;
    }
}

```

## Stream流的收集操作

> 对数据使用Stream流的方式操作完毕后，我想把流中的数据收集到集合中，该怎么办呢？

* Stream流的收集方法
  * R collect(Collector collector)
  * 但是这个收集方法的参数是一个Collector接口

* 工具类Collectors提供了具体的收集方式
  * public static <T> Collector toList()：把元素收集到List集合中
  * public static <T> Collector toSet()：把元素收集到Set集合中
  * pulic static Collector toMap(Function keyMapper,Function valueMapper)：把元素收集到Map集合中

```java
public class StreamDemo10 {
    public static void main(String[] args) {
        //创建List集合
        ArrayList<String> list = new ArrayList<>();
        list.add("林青霞");
        list.add("张曼玉");
        list.add("王祖贤");
        list.add("柳岩");

        //需求：得到名字为3个字的流
        Stream<String> listStream = list.stream().filter(s -> s.length() == 3);

        //需求：把使用Stream流操作完毕的数据收集到List集合中并遍历
        List<String> names = listStream.collect(Collectors.toList());

        for (String name:names){
            System.out.println(name);
        }

        //创建Set集合
        HashSet<Integer> set = new HashSet<>();
        set.add(10);
        set.add(20);
        set.add(30);
        set.add(33);
        set.add(35);

        //需求：得到年龄大于25的流
        Stream<Integer> setStream = set.stream().filter(age -> age> 25);
        //需求：把使用Stream流操作完毕的数据收集到Set集合中并遍历

        Set<Integer> ages = setStream.collect(Collectors.toSet());
        for (Integer age : ages){
            System.out.println(age);
        }


        //定义一个字符串数组，每一个字符串数据由姓名和年龄数据组合而成
        String[] strArray = {"林青霞,30","张曼玉,35","王祖贤,33","柳岩,35"};

        //需求：得到字符串中年龄数据大于28的流
        Stream<String> strArray1 = Stream.of(strArray);
        Stream<String> stringStream = strArray1.filter(s -> Integer.parseInt(s.split(",")[1]) > 28);
        //需求：把使用Stream流操作完毕的数据收集到Map集合中并遍历，字符串中的姓名作键，年龄做值
        Map<String, String> map = stringStream.collect(Collectors.toMap((s -> s.split(",")[0]), (s -> s.split(",")[1])));

        Set<Map.Entry<String, String>> es = map.entrySet();
        for (Map.Entry<String, String> me:es){
            System.out.println(me);
        }

    }
}

```

