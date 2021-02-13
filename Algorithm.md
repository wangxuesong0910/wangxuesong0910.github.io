---
title: "Algorithm"
date: 2020-06-11T21:48:07+08:00
draft: true
---

# 算法的日常整理归纳

## 查找



#### 二分查找（折半查找）的java实现过程

* 查找过程为：从表中间定义一个中间值，如果中间值与要查找的内容相等 ，则查找成功，如果查找值大于或小于中间值，则在中间值相应的那一半中进行查找，这样每次查找都会将范围缩小一半，与顺序查找相比，提高了查找效率

```java
//条件：顺序存储的线性表，元素有序排列
int[] arr = new int[]{1,2,3,4,5}

int low = 0;
int high = arr.length;
int desc = 3;//定义一个变量为要查找的值
while(low <= high){
    mid = (low+high)/2
    if(desc = arr[mid]){
        System.out.print("找到了位置为："+mid)
    }else if(desc < arr[mid]){
        high = mid - 1;
    }else{
        low = mid + 1 ;
    }
}
```

***

## 排序

### 交换排序

<span style="background:yellow">交换排序的基本思想：两两比较待排序记录的关键字，一旦发现两个记录不满足排序要求时，就进行交换直到整个序列全部满足要求为止。</span>

#### 冒泡排序的java实现过程

> 通过两两比较相邻记录的关键字，如果发生逆序则交换，关键字小的如气泡一样上升（左移），关键字打的如石头一样下降（右移）

```java
int[] arr = new int[]{1,2,3,4,5}

for(int i = 0;i < arr.length -1;i++){
   for(int j = 0;j< arr.length -i -1;j++){
      if(arr[j] > arr[j+1]){
        int temp = arr[j];
        arr[j] = arr[j+1];
        arr[j+1] = temp;
      }
   }
}
```

***

#### 快速排序

<span style= "background:#ffe4b5">由冒泡排序改进而得，冒泡排序每次交换只能消除一个逆序，快速排序依次交换可能消除多个逆序</span>

> 