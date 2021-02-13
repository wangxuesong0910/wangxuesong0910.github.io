# Java的队列(Queue)

* 队列是一个典型的先进先出(FIFO)的容器，即：从容器的一端放入事物，从另一端取出，并且事物放入容器的顺序与取出的顺序是相同的
* 队列在并发编程中特别重要：常被当做一种可靠的将对象从程序的某个区域传输到另一个区域的途径
* 意义：安全地将对象从一个任务传输给另一个任务
* LinkedList提供了方法以支持队列的行为，因为它实现了Queue接口

## Queque接口中的方法

| Modifier and Type | 方法         | 描述                                                         |
| ----------------- | ------------ | ------------------------------------------------------------ |
| `boolean`         | `add(E e)`   | 将指定的元素插入到此队列中，如果可以立即执行此操作而不违反容量限制， `true`成功返回  `true` ，如果当前没有可用的空间，则抛出 `IllegalStateException` 。 |
| `E`               | `element()`  | 检索，但不删除，这个队列的头。                               |
| `boolean`         | `offer(E e)` | 如果在不违反容量限制的情况下立即执行，则将指定的元素插入到此队列中。 |
| `E`               | `peek()`     | 检索但不删除此队列的头，如果此队列为空，则返回 `null` 。     |
| `E`               | `poll()`     | 检索并删除此队列的头部，如果此队列为空，则返回 `null` 。     |
| `E`               | `remove()`   | 检索并删除此队列的头。                                       |

## LinkedList

* LiskedList提供了方法以支持队列的行为，并且实现了Queue接口
* LinkedList中的常用方法

| Modifier and Type |     方法     |                       描述                       |
| :---------------: | :----------: | :----------------------------------------------: |
|     `boolean`     | `offer(E e)` | 将指定的元素添加为此列表的尾部（最后一个元素）。 |
|        `E`        | `element()`  |      检索但不删除此列表的头（第一个元素）。      |
|        `E`        |   `peek()`   |      检索但不删除此列表的头（第一个元素）。      |
|        `E`        |   `poll()`   |       检索并删除此列表的头（第一个元素）。       |
|        `E`        |  `remove()`  |       检索并删除此列表的头（第一个元素）。       |

* peek()方法在队列为空时返回null，而element()会抛出NoSuchElementException异常
* poll()在队列为空时返回null，而remove()会抛出NoSuchElementException异常

```java
public class QuequeDemo {
    public static void printQ(Queue que){
        while (que.peek()!= null){
            System.out.println(que.remove()+" ");
        }
    }

    public static void main(String[] args) {
        //Queue接口窄化了对LinkedList的方法的访问权限，使得只有恰当的方法才可以使用，但是能够使用的LinkedList方法会变少
        //虽然可以将queue转回LinkedList，但是不鼓励这么做（Why？）
        Queue<Integer> que = new LinkedList<>();
        Random random = new Random(47);
        for (int i = 0; i <10 ; i++) {
            que.offer(random.nextInt(i+10));
            printQ(que);
        }

        Queue<Character> qc = new LinkedList<>();
        for (char c : "Bubudeai".toCharArray()){
            qc.offer(c);
            printQ(qc);
        }
    }
}
```

## PriorityQueue

* 队列规则：指在给定一组队列中的元素的情况下，确定下一个弹出队列的元素的规则
* 先进先出声明的是下一个元素应该是等待时间最长的元素
* 优先级队列：声明下一个弹出元素是最需要的元素（具有最高优先级）

```java
public class PriorityQueueDemo {
    public static void main(String[] args) {
        PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();
        Random random = new Random(47);
        for (int i = 0; i <10 ; i++) {
            //PriorityQueue上调用offer()方法来插入一个对象时，这个对象会在队列中被排序（依赖于具体实现的排序） P425
            priorityQueue.offer(random.nextInt(i+10));
            QuequeDemo.printQ(priorityQueue);
        }

        List<Integer> ints = Arrays.asList(25, 22, 20, 18, 14, 9, 1, 3, 1, 2, 3, 9, 14, 18, 51, 23, 25);

        //priorityQueue = new PriorityQueue<>(ints);
        PriorityQueue<Integer> integers = new PriorityQueue<>(ints);
        QuequeDemo.printQ(integers);
        PriorityQueue<Integer> ints1 = new PriorityQueue<>(ints.size(), Collections.reverseOrder());
        QuequeDemo.printQ(ints1);
    }
}

```

* 重复是允许的，最小的值拥有最高的优先级

## 练习

```java
/**
 * 创建一个类，它包含一个Integer，
 * 其值通过使用java.util.Random被初始化为0到100之间的某个值，
 * 使用这个Integer域来实现Comparable。用这个类的对象来填充PriorityQueue，
 * 然后使用poll()抽取这些值以展示该队列将按照我们预期的顺序产生这些值
 */
class IntegerTest implements Comparable<IntegerTest>{
   Random r =  new Random();
   Integer i = r.nextInt(100);
    @Override
    public int compareTo(IntegerTest arg) {
        int i = this.i - arg.i;
        return i>0? 1:i==0? 0:-1;
    }

    public String toString() {
        return Integer.toString(i);
    }
}

public class Test11{

    public static void main(String[] args) {
        PriorityQueue<IntegerTest> it = new PriorityQueue<>();
        for (int i = 0; i <10 ; i++) {
            
        it.offer(new IntegerTest());
        }

        for (int i = 0; i <it.size() ; i++) {

            System.out.println(it.poll());
        }

    }


}

```

