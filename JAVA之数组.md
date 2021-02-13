---
title: "JAVA之数组"
date: 2020-05-25T02:59:26+08:00
draft: true
---

## JAVA之数组

***

#### 两种初始化数组的方式

1. 动态初始化

```java
//动态初始化只定义了数组的长度，不指定数组的值
int[] arr = new int[3];
```



1. 静态初始化

```java
//静态初始化从一开始就已经定义好了数组的值
int[] arr = new int[]{1,2,3};

//简化写法
int[] arr = {1,2,3};
```

***

#### 数组的默认初始化值

1. （byte,short,long,int）类型：0
2. （double,float）类型：0.0
3. String类型：空
4. Boolean类型：false

***

#### 数组的遍历

```java
//动态初始化一个数组
int[] arr = new int[3];

//遍历操作
for(int i = 0;i<arr.length;i++){
    System.out.println(arr[i]);
}
```

***

#### 求最大值问题

```java
//动态初始化一个数组
int[] arr = new int[3];

//定义一个变量用于存储最大值，赋值给其索引为0位置上的元素,赋值int类型即可
int max = arr[0];

//遍历操作
for(int i = 0;i<arr.length;i++){
// 将当前数组元素与max比较，如果大于，则将当前元素赋值给max
    if(arr[i]>max){
    max = arr[i];
    }
}

//输出最大值
System.out.println(arr[i]);
```

## Java中内存分配

* 栈内存：存储局部变量
  * 定义在方法中的变量
  * 使用完毕，立即消失

* 堆内存：存储new出来的内容（实体，对象）

  * 数组在初始化时，会为存储空间添加默认值

  | 整数 | 浮点数 | 布尔  |  字符  | 引用数据类型 |
  | :--: | :----: | :---: | :----: | :----------: |
  |  0   |  0.0   | false | 空字符 |     null     |

  * 每一个new出来的东西都有一个地址值
  * 使用完毕，会在垃圾回收器空闲时被回收

