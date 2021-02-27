title: Java学习总结--第十五章 异常和断言
date: 2016-01-30 16:41:58
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
本章介绍应用异常处理来处理运行错误，以及应用断言来确保程序的正确性。

### 异常和异常类型
- 运行会引起`异常（exception）`。异常是指程序运行中出现的时间，它中断正常的程序控制流。
- `异常处理（exception handing）`：Java给程序员提供的稳妥处理运行错误的功能。
- 异常类
  + Java的异常是Throwable派生类的一个实例。		
  + 通过扩展Throwable或它的子类，可以创建自己的异常类。
  + 异常类可以分为三种主要类型：系统错误、异常和运行异常。
    * `系统错误（system error）`是由Java虚拟机抛出并在Error类中描述的。Error类描述内部的系统错误。此类错误程序员一般无法处理。（LinkageError、VirtualMachineError、AWTError）
    * `异常（exception）`是由Exception类描述的。Exception类描述由程序和外部环境引起的错误，这些错误能通过程序捕获和处理。（classNotFoundException、RuntimeException、···）
    * `运行异常（runtime exception）`是由RuntimeException类描述的。RuntimeException类描述编程错误，比如不合适的转换、访问一个越界数组或数值错误等。（ArithmeticException、NullPointerException、IndexOutOfBoundsException、···）
- 必检异常和免检异常
  + RuntimeException、Error以及它们的子类都称为`免检异常（unchecked exception）`。所有其他异常都成为`必检异常（checked exception）`。
  + 必检异常编译器会强制程序员检查并处理它们。免检异常反映程序设计中不可重获的逻辑错误。
  + Java语言不允许编写捕获或声明免检异常的代码。

### 异常处理
Java的异常模型基于三种操作：`声明异常（declaring an exception）`、`抛出异常（throwing an exception）`和`捕获异常（catching an exception）`。
- 声明异常：每个方法都必须说明它可能抛出的必检异常的类型。
  + 在Java中使用关键字`throws`声明异常，使用关键字`throw`抛出异常。
- 抛出异常：程序检查到一个错误后，创建一个适当类型异常的实例并抛出它。
  + 一个方法总能抛出免检异常。
- 捕获异常：使用`try-catch`语句块捕获并处理异常。
  ```java
  try {
  	//some statments
  } catch(Exception e) {
  	//do
  }
  ```
  + 处理异常的代码称为`异常处理器（exception handler）`。寻找处理器的整个过程称为捕获异常。
  + 如果try语句块中某条语句抛出了异常，Java就会跳过try语句块中剩下的语句，开始为该异常搜索异常处理器。从第一个到最后一个逐个检查catch语句，看是否有某个catch字句中的异常类实例与该异常的类型匹配。如果有，就将该异常对象赋值给所声明的变量，执行catch子句中的代码。如果没有发现异常处理器，Java退出这个方法，把异常传递给调用该方法的方法，继续同样的过程来查找处理器。如果在调用的方法链中找不到相应的处理器，程序终止并在监视器上输出错误信息。
  + 一个异常对象包含有关异常的有价值信息，可以利用java.lang.Throwable类中的下列实例方法获取异常的信息：
    * public String getMessage() 返回Throwable对象的详细信息
    * public String toString() 返回3个字符串合起来的串，它们分别是 1）异常类名的全称。2）“：”（一个冒号和空格）。3）getMessage()方法
    * public void printStackTrace() 在控制台输出Throwable对象及其踪迹信息
- 方法是按照线程执行的。如果一个线程发生异常没有得到处理，该线程中止，但是程序中的其他线程不受影响。

### 重新抛出异常
- 当一个方法出现异常是，如果没有捕获异常，该方法就会立即退出。如果方法在退出之前需要执行某些任务，应该在该方法中捕获异常，然后按如下结构将异常重新抛出，交给调用它的方法：
```java
try {
	statements;
} catch(TheException ex) {
	perform operations before exits;
	throw ex;
}
```

### finally子句
- 有时，不论异常是否出现或者是否被捕获，都希望执行某些代码。可以使用如下结构实现：
```java
try {
	statements;
} catch (TheException ex) {
	handling ex;
} finally {
	finalStatements;
}
```
**使用finally子句时可能会忽略catch子句。**

### 何时使用异常
当必须处理不可预料的错误时应该使用try-catch语句块，不要用try-catch块处理简单的、可预料的情况。

### 创建自己的异常类
可以通过扩展Exception类或其子类来创建自己的异常类。

### 断言
`断言（assertion）`是Java的一个语句，它允许对程序提出一个判断（假设）。断言包含一个布尔表达式，在程序运行中它应该是真。
- 断言用于确保程序的正确性，避免逻辑错误。
- 声明断言：
```java
assert assertion;
or
assert assertion:detailMessage;
```
其中assertion是一个布尔表达式，detailMessage是一个基本类型值或一个对象值。
- 运行带断言的程序
  + 默认情况下，断言在运行时不起作用。为使它们有效，使用开关`-enableassertions`或其缩写`-ea`:
  ```java
  java -ea xxx
  ```
  + 断言可以有选择地激活，在类层次或包层次起作用或不起作用。使它不起作用的开关是`-disableassertions`,或其缩写`-da`。
  ```java
  java -ea:package1 -da:Class1 xxx
  ```

### 使用异常处理或断言
- 不应该使用断言代替异常处理。异常处理用于在程序运行期间处理非常环境，断言是要确保程序的正确性。
- 异常处理针对程序的健壮性，而断言涉及程序的正确性。
- 不要使用断言检测public方法的参数。传给public方法的有效参数被认为是方法合约的一部分。
- 使用断言进一步确认假设。这将加强对程序正确的的确认。
- 将断言放到没有缺省情况的switch语句中。

---

## 复习小结
- 从一个通用父类可以派生出多种异常类。如果一个catch子句可以捕获一个父类的异常对象，它就能捕获那个父类所有子类的异常对象。
- 在catch字句中指定异常的顺序是非常重要的。一般遵循从子类异常到父类异常的顺序。

---

## 编程练习
习题15.5源代码见我的Github： [chapter15](https://github.com/whtis/Java-Exercises/tree/master/chapter15/src)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>