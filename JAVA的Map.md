---
		title: "JAVA的Map"
date: 2020-07-11T14:42:53+08:00
draft: true
---

# Map

## Map集合概述和使用

* Map集合概述
  * Interface Map<K , V>  K：键的类型； V：值的类型
  * 将键映射到值的对象；不能包含重复的键；每个键可以映射到最多一个值
  * 举例：学生的学号和姓名
    * 001   王
    * 002   刘
    * 003   李

* 创建Map集合的对象
  * 多态的方式
  * 具体的实现类HashMap

```java
public class Demo{
  public static void main(String[] args){
    Map<String,String> map = new HashMap<String,String>();//多态方式创建对象
    
    //V put (K key, V value)将指定的值与该映射中的指定键相关联
    
    map.put("001","王");
    map.put("002","刘");
    map.put("003","李");
    map.put("001","王");//当键"001"出现第二次的时候会修改第一次存储进键"001"的值，保证了键的唯一性（由HashMap保证，根据哈希表）
  }
}
```

***

## Map集合的基本功能

| 方法名                              | 说明                                 |
| ----------------------------------- | ------------------------------------ |
| V  put(K key, V value)              | 添加元素                             |
| V  remove(Object key)               | 根据键删除键值对元素                 |
| void clear()                        | 移除所有键值对元素                   |
| boolean containsKey(Object key)     | 判断集合是否包含指定的键             |
| boolean containsValue(Object value) | 判断集合是否包含指定的值             |
| boolean isEmpty()                   | 判断集合是否为空                     |
| int size()                          | 集合的长度，也就是集合中键值对的个数 |

### Map集合的获取功能

| 方法名                         | 说明                     |
| ------------------------------ | ------------------------ |
| V get(Object key)              | 根据键获取值             |
| Set<K> keySet()                | 获取所有键的集合         |
| Collection<V> values()         | 获取所有值的集合         |
| Set<Map.Entry<K,V>> entrySet() | 获取所有键值对对象的集合 |

### Map集合的遍历（方式一）

* Map存储的元素都是成对出现的，所以我们把Map看成是一个夫妻对的集合，遍历思路如下：
  * 把所有的丈夫给集中起来
  * 遍历丈夫的集合，获取到每一个丈夫
  * 根据丈夫去寻找对应的妻子

* 转换为Map集合的操作如下：
  * 获取所有键的集合，用keySet()方法实现
  * 遍历键的集合，获取到每一个键，用增强for实现
  * 根据键去找值，用get(Object key)方法实现

```java
public class Demo{
  public static void main(String[] args){
    Map<String,String> map = new HashMap<String,String>();
    
    map.put("11","www");
    map.put("22","xxx");
    map.put("33","sss");
    
    
    Set<String> ss = map.keySet();//获取所有键的集合
    
   //遍历
    for(String s:ss){
      String value = map.get(s);//用get方法获取每个键对应的值
      System.out.println(s+","+value);
    }
  }
}
```

### Map集合的遍历（方式二）

* 如果我们把Map看成是一个夫妻对的集合，遍历思路如下：
  * 获取所有结婚证的集合
  * 遍历结婚证的集合，得到每一个结婚证
  * 根据结婚证获取丈夫和妻子

* 转换为Map集合中的操作：
  * 获取所有键值对对象的集合
    * Set<Map.Entry<K,V>> entrySet()：获取所有键值对对象的集合
  * 遍历键值对对象的集合，得到每一个键值对对象
    * 用增强for实现，得到每一个Map.Entry
  * 根据键值对对象获取键和值
    * 用getKey()得到键
    * 用getValue()得到值

```java
public class Demo{
  public static void main(String[] args){
    Map<String,String> map = new HashMap<String,String>();
    
    map.put("11","www");
    map.put("22","xxx");
    map.put("33","sss");
    
    //获取所有键值对对象的集合：Set<Map.Entry<String,String>> entrySet();
    Set<Map.Entry<String,String>> es = map.entrySet();
    
    //遍历
    for(Map.Entry<String,String me: es){
      System.out.println(me.getKey()+","+me.getValue());//根据键值对对象获取键和值
    }
  }
}
```

***

# 案例

```java
//需求：键盘录入一个字符串，要求统计字符串中每个字符出现的次数
//例：键盘录入"aazzbb"   控制台输出："a(2)b(3)z(2)"

public class Demo{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入字符串");
        String s = sc.nextLine();
        //创建HashMap集合
        HashMap<Character,Integer> cih = new HashMap<>();
        //遍历字符串，得到每一个字符
        for(int i = 0;i<s.length();i++){
            char c = s.charAt(i);//获取到每一个字符，变量存储
            //获取字符对应的value值
            Integer value = cih.get(c);
            //判断：如果value为空，说明集合中不存在，将该字符作为键，1作为其值存储
            if(value == null){
                cih.put(c,1);
            }else{
                //否则说明集合中存在该值，value值做++操作并存储
                value++;
                cih.put(c.value);
            }
        }
        
        StringBuffer sb = new StringBuffer();
        
        //获取所有键值对对象的集合：Set<Map.Entry<String,String>> entrySet();
        Set<Map.Entry<String,String>> es = cih.entrySet();
        
        //遍历
        for(Map.Entry<String,String> me : es){
            sb.append(me.getKey()).append("(").append(me.getValue()).append(")");
        }
        
        System.out.println(sb.toString());
    }
}
```

