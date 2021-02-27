title: Java学习总结--第九章 抽象类和接口
date: 2016-01-28 17:03:02
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
在继承的层次结构中，子类的出现使类变得越来越具体，而子类的父类就变得更一般、更通用。有时将一个父类设计得非常抽象，以至于它没有具体的实例，这样的类称为`抽象类（abstract class）`。
一个子类只能从一个父类继承，如果想要使其继承多个父类，可以使用`接口（interface）`来达到`多重继承（multiple inheritance）`的效果。
本章介绍抽象类和接口，并讨论如何使用基本数据类型值的包装类。

### 抽象类
- 抽象类使用关键字`abstract`修饰。
- 抽象类中可以存在抽象方法，抽象方法也使用关键字`abstract`修饰。
- 非抽象类不能包含抽象方法，在一个由抽象类扩展出来的非抽象类中，所有抽象方法都必须实现。
- 抽象类不能用new运算符实例化，但仍然可以定义它的构造方法。
- 包含抽象方法的类必须是抽象的，但抽象类中可以存在非抽象方法。
- 子类可以声明为抽象的，即使他的父类是具体的。
- 不能用new运算符创建抽象类的实例，但是，抽象类可以用作数据类型。例如：
```java
GeometricObject[] geo = new GeometricObject[10];
```

### 日历类Calendar和公历类GregorianCalendar
略。

### 接口
- `接口(interface)`是一种与类相似的结构，只包含常量和抽象方法。抽象类和接口相似，但是抽象类处理包含常量和抽象方法外，还可以包含变量和具体方法。
- 使用关键字`implements`可以实现接口。
- 接口与抽象类
  + 在接口中，数据必须是常量，而抽象类可以有非常量的数据域。
  + 接口中的每个方法只有一个头标志，没有实现部分，抽象类可以有具体的方法。
  **在接口中，所有的数据域都是public final static的，所有的方法都是public abstract的，在书写接口时可以省略这部分内容。**
  + 一个接口只能扩展其他接口，不能扩展类。一个类可以扩展它的父类并实现多个接口。
- 抽象类和接口都可以用来模拟共同特征。
  + `强是关系(Strong is-a relationship)`：明显地描述了父子关系应该使用类模拟。
  + `弱是关系(Weak is-a relationship)`：也叫`类属关系（is-kind of relationship）`，是指对象拥有某种属性。用接口来模拟。
- 可克隆接口Cloneable
  + Cloneable接口在java.lang包中如下：
  ```java
  public interface Cloneable {

  }
  ```
  + 体为空的接口称为`标记接口（marker interface）`。标记接口不包含常量或方法，它用于标明一个类具有某种必备的属性。
- 浅复制和深复制
  + `浅复制(shallow copy)`：如果域是一个对象，复制的是域的引用而不是域的内容。
  + `深复制(deep copy)`: 复制域的内容。需要重写clone()方法。

### 将基本数据类型值处理为对象
- 在Java中，基本类型不作为对象使用，这样做是效率原因。同时，Java也将基本数据类型并入对象或包装成对象，方便使用。对应的类称为`包装类（wrapper class）`。使用包装对象代替基本类型变量，可以进行一般程序设计。
- 包装类的名称与对应的基本类型一样，但是第一个字母大写。Integer和Character例外。
- 包装类没有无参构造方法；所有包装类的实例都是永久的。
- 数值包装类的常量
  + MAX_VALUE
  + MIN_VALUE
- 静态方法`valueOf()`创建一个新的对象，并将它初始化为指定字符串表示的值。
```java
Double doubleObject = Double.valueOf("12.4");
```

### 基本类型和包装类之间的自动转换
- 二者自JDK 1.5就可以自动转换。
- 将基本类型的值转换为包装类对象，称为`装箱（boxing）`，相反的过程称为`开箱（unboxing）`。

---

## 复习小结
- 接口仅定义了规范，实现接口的好处有：
  1、表示多个实现类之间的弱是关系
  2、接口可以同一标准
  3、接口可以快速分离工作内容
  4、接口有利于程序扩展
- 实现接口，必须实现接口中所有的方法。

---

## 编程练习
习题9.1 9.2 9.3 9.4 9.6 9.8 9.9 9.10程序源代码见我的Github： [chapter9](https://github.com/whtis/Java-Exercises/tree/master/chapter9/src)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>