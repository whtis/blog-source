title: Java学习总结--第五章 数组
date: 2016-01-20 19:59:25
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
本章介绍了数组的基本知识。数组用于存储一个元素个数和类型固定的有序集，并提供对这些数据进行处理的各种方法。

### 数组
- 声明数组： 数据类型[] 数组名
- 数组创建后，数组元素通过下标来访问。数组下标是`基于0的（0-based）`，因此数组的下表最大值是arrayObject.length-1。
- 增强的for循环可以在不用下标变量的情况下访问整个数组，用法如下:
  ```java
  for(elementType value:arrayRefVar) {
     //process the value  
  }
  ```
- 数组初始化：数据类型[] 数组变量 = {直接量1，直接量2，···，直接量n}；
  数组初始化语句中不使用运算符new。使用时，必须将声明、创建、初始化数组都放在一个语句中。

### 数组的复制
- 可以直接使用赋值语句（=）进行数组的复制，这样操作仅能够将数组的引用进行复制。
- 复制数组中的内容有以下三种方法:
  + 用循环语句分别复制数组的每一个元素。
  + 使用System类中的静态方法arraycopy（）方法。
  + 使用clone（）方法复制数组。

### 数组的排序
- 选择排序法
- 冒泡排序法
- 插入排序法
具体算法见[排序的几种写法](http://www.whtis.com/2015/12/03/java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-%E6%8E%92%E5%BA%8F%E7%9A%84%E5%87%A0%E7%A7%8D%E5%86%99%E6%B3%95/)
**在java.util.Array类中，Java提供了几个排序的重载方法，可以用来对int型、double型、char型、short型、long型和float型数组进行排序。**

### 数组的查找
- 线性查找
```java
private static int listSearch(int[] lists, int key) {
        for (int i = 0; i < lists.length; i++) {
            if (lists[i] == key) {
                return i;
            }
        }
        return -1;
    }
```
- 二分法查找：前提是数组元素必须已经排序。
  + 二分法的迭代实现
  ```java
  private static int binarySearch(int[] lists, int key, int low, int high) {
        while (low <= high) {
            if (key == lists[(high + low) / 2]) {
                return (high + low) / 2;
            } else if (key > lists[(high + low) / 2]) {
                low = ((high + low) / 2) + 1;
            } else if (key < lists[(high + low) / 2]) {
                high = ((high + low) / 2) - 1;
            }
        }
        return -low - 1;
    }
  ```
  + 二分法的递归实现
  ```java
  private static int recursiveBinarySearch(int[] lists, int key, int low, int high) {
        if (low > high) {
            return -low - 1;
        }
        int mid = (low + high) / 2;
        if (key < lists[mid]) {
            return recursiveBinarySearch(lists, key, low, mid - 1);
        } else if (key == lists[mid]) {
            return mid;
        } else
            return recursiveBinarySearch(lists, key, mid + 1, high);
    }
  ```

### 多维数组
- 声明：数据类型[][] 数组引用变量
-数组的长度：数组的长度是指数组中元素的个数。`n`维数组是由`k`个`n-1`维数组构成，那么n维数组的个数就是`k`。例如：
x = new int[2][3][5],则x[0]和x[1]是二维数组，x的长度就是2。
- 锯齿数组
二维数组中，每一行本身是一个数组。因而各行就可以有不同的长度，这样的数组称为锯齿数组。例如：
```java
int[][] x = {
	{1,2,3,4,5},
	{2,3,4,5},
	{3,4,5},
	{4,5},
	{5}
}
```
---

## 复习小结
- 数组变量只存储着数组的引用。
- 越界访问数组是常见的编程错误。它出现错误时的Exception是ArrayIndexOutOfBoundsException，通常称为`过一错误（off-by-one error）`
- 使用语法new int[5][]创建数组时，第一个下标必须给定。new int[][]是错误的。

---

## 编程练习
习题5.5 5.7 5.12 5.14 5.15 5.16 5.25源代码见我的Github： [chapter5](https://github.com/whtis/Java-Exercises/tree/master/chapter5/src)




---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>