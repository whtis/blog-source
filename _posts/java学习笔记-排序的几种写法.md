title: java学习笔记--排序的几种写法
date: 2015-12-03 20:00:08
categories: java
tags: [java,原创,java学习笔记]
---

最近学到数组这章，最重要的就是对数组中的元素进行排序。总结一下排序的几种写法。
*java.util.array类中提供了排序的重载方法，可以直接调用相关方法进行排序，详见[API](http://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#sort-byte:A-)*

- 选择排序
 * 写法一：遍历数组，找到数组中最大的元素，与数组最后的元素进行交换。如此进行n次，数组中的元素就按照升序排列完成了。
 ```java
 for (int i = lists.length - 1; i >= 1; i--) {
            double max = lists[0];
            int index = 0;
            for (int j = 0; j <= i; j++) {
                if (max < lists[j]) {
                    max = lists[j];
                    index = j;
                }
            }

            //交换list[i]和max所在的list[index]的值
            if (index != i) {
                lists[index] = lists[i];
                lists[i] = max;
            }
        }
 ```
 * 写法二：写法一的递归实现
 ```java
 void selectionSort(double[] lists, int length) {
    //length = lists.length -1
        double max = lists[0];
        int index = 0;
        for (int i = 0; i <= length; i++) {
            if (max < lists[i]) {
                max = lists[i];
                index = i;
            }
        }
        if (index != length) {
            lists[index] = lists[length];
            lists[length] = max;
        }
        if (length > 0) {
            length--;
            selectionSort(lists, length);
        }
    }
 ```
 * 写法三：写法一的改进，优点是升序和降序排列只需要简单的修改。
 ```java
 for (int i = 0; i < lists.length; i++) {
            for (int j = lists.length - 1; j > i; j--) {
                if (lists[i] > lists[j]) {
                    double tmp = lists[i];
                    lists[i] = lists[j];
                    lists[j] = tmp;
                }
            }
        }
 ```
** 若要实现降序排列，只需将第三行的`>`改为`<`即可。**
 - 冒泡排序
   冒泡排序又称为下沉排序，基本思想是在每次的遍历中，连续对相邻元素进行比较，以达到按照特定顺序排序的目的。
```java
 boolean changed = true;  //用于控制数组遍历的次数（至多lists.length-1次）
        do {
            changed = false;
            for (int i = 0; i < lists.length - 1; i++) {
                if (lists[i] > lists[i + 1]) {
                    double tmp = lists[i];
                    lists[i] = lists[i + 1];
                    lists[i + 1] = tmp;
                    changed = true;
                }
            }
        } while (changed);
```
** 若要实现降序排列，只需将第三行的`>`改为`<`即可。**
- 插入排序
  基本思路是在已排好序的子数列中反复插入一个未排序的元素，直到整个数列全部排好。
```
 for (int i = 1; i < lists.length; i++) {
            double currentElement = lists[i];
            int k = i - 1;
            while (k >= 0 && lists[k] > currentElement) {
                double tmp = lists[k + 1];
                lists[k + 1] = lists[k];
                lists[k] = tmp;
                k--;
            }
        }
```



---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>