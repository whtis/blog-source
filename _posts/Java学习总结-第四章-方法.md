title: Java学习总结--第四章 方法
date: 2016-01-11 22:16:41
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
本章介绍了如何编写带返回值和不带返回值的方法以及如何调用含参/不含参的方法。采用这些规范可以编写递归方法，最后介绍如何使用方法抽象进行程序设计。

### 基本方法的语法结构：
```java
修饰符 返回值类型 方法名 （参数列表）
{
    //方法体
}
```

### 调用方法
创建方法时，定义了方法干什么。要使用方法，必须`调用`（call）或`引用`（invoke）它。
- 调用堆栈：每当调用一个方法时，系统将参数、局部变量和系统寄存器存储在一个内存区域中。这个内存区域称为`堆栈`（stack）。它使用后进先出的方式存储数据。

### 参数传递
- 参数传递需要注意`参数顺序匹配`。
- Java中的参数传递，如果是基本数据类型，传递是值本身，如果是引用数据类型，传递的是这个引用。
**需要注意的是，实参将参数传给形参，这种传递方式称为`值传递`（pass by value）。无论形参在方法中如何改变，该实参的值不受影响。（可以通俗的理解为：实参传递给形参的这个过程，是copy了一份给形参）。**

### 重载方法
- 主要用于不同的构造方法。
- 重载方法可使程序清晰易读。执行相似任务的方法应该给予相同的名称。
- 有时一个方法调用会有两个或更多可能的匹配，编译器无法判断哪个更为合适。这成为`歧义匹配。`歧义匹配是编译错误。

### 方法抽象
- 方法抽象是把方法的应用同实现分离开来。
- 在不知道方法如何实现的情况下也可以使用方法，这就是方法的`信息隐藏`或`封装`。

### Math类的常用方法
- 三角函数方法：sin、cos、tan、asin、acos、atan、toRadians、toDegrees······
- 指数函数方法：exp、log 
- 服务型方法：pow、sqrt
- 常量：Math.PI、Math.E
- 取整方法：ceil、floor、rint、round
- min、max、abs
- random方法
  +  random方法生成大于等于0.0小于1.0的double型随机数（0<Math.random()<1.0）。该方法十分有用，可以用它来生成任意范围的随机数。
  +  可以生成任意两个字符ch1和ch2(ch1<ch2）之间之间的随机字符：
  ```java
  (char)(ch1+Math.random()*(ch2-ch1+1)
  ```
### 递归
- 递归是函数直接或间接调用自己的过程。
- 所有的递归方法都用下列特征：
  + 有一个或多个初始状态（最简单情形）用于中断递归。
  + 每次递归调用都简化原始问题，使它越来越接近初始状态，直到达到初始状态。
- 递归的两个实例：
  + 计算斐波那契数
  ```java
  static long fib(int index) {
        if (index == 0) {
            return 0;
        } else if (index == 1) {
            return 1;
        } else {
            return fib(index - 1) + fib(index - 2);
        }
    }
  ```
  + 汉诺塔问题
  ```java
  private static void moveDisks(int n, char fromTower, char ToTower, char auxTower) {
        if (n == 1) {
            System.out.println("move disk " + n + " from " + fromTower + " to " + ToTower);
        } else {
            moveDisks(n - 1, fromTower, auxTower, ToTower);
            System.out.println("move disk " + n + " from " + fromTower + " to " + ToTower);
            moveDisks(n - 1, auxTower, ToTower, fromTower);
        }
  ```
- 递归与迭代
  + 递归是程序控制的另一种形式，本质上是不用循环控制的重复。
  + 递归和迭代相比，会耗费太多的时间并占用太多的内存。
  + 能用递归解决的问题都可以用迭代完成，递归仅在某些能够简化问题解法的情况下使用。

### 包
- 包用于对类进行组织。
- 包是有层次的，使用包需要先引入。
- 使用包可以避免命名冲突。
  
---
  
## 复习小结
- 方法是一个提供程序模块化和可重用性的结构。
- 方法中不可以再定义方法。  
- 歧义匹配的最典型例子就是两个方法的名称和参数列表都相同，但其返回值或者修饰符不同。
```java
public static void method(int x) {
    
}
public static int method(int y) {
    return y;
}
```

---

## 编程练习
习题4.10 4.11 4.12 4.13 4.15 4.16 4.17 4.20 4.21 4.22 4.23 4.24 4.25源代码见我的Github： [chapter4](https://github.com/whtis/Java-Exercises/tree/master/chapter4/src)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>