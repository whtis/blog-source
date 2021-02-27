title: Java学习总结--第十章 面向对象建模
date: 2016-01-29 18:48:33
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
本章初步介绍使用面向对象的方法开发软件系统，学习类的设计原则。

### 软件开发过程
- 软件开发步骤：需求分析、系统分析、设计、实现、测试、发行应用和维护。

### 分析类之间的关系
- `关联(association)`：是一种描述两个类之间行为的一般二元关系。
- 聚集和包容
  + `聚集(aggregation)`：是一种特殊的关联形式，表示两个类之间的所属关系。聚集模拟`具有（has-a）`关系。
  + 如果一个对象被一个聚集对象所专有，它和聚集对象之间的关系就称为`包容（composition）`。
- 继承
`继承(inheritance)`模拟两个类之间“是”（is-a）的关系。`强是(strong is-a)`关系描述两个类之间的直接继承关系，可以用类的继承表示。`弱是(weak is-a)`关系描述一个类具有某种属性，可以用接口表示。

### 类的设计原则
- 设计一个类
  + 一个类应该描述一个单一的实体，类的所有操作应该在逻辑上相互配合，支持一个共同的目标。
  + 类经常是为了多种不同用户的使用而设计的。
  + 类是为了可重用而设计的，用户可以在不同的组合、不同的顺序和不同的环境中联合使用类。
  + 在可能的情况下，应该提供公用的无参构造方法，并且覆盖定义在Object类中的equals方法和toString方法。如果覆盖了equals方法，也应该覆盖hashcode方法。（原因是两个相等的对象必须拥有相等的散列码）
  + 给类、数据域以及方法选择有意义的名字。建议将数据的声明置于构造方法之前，将构造方法置于方法之前。
- 使用可见性修饰符public、protected和private
  + 一个类可以提供两种合约：对类的使用者和对类的扩展者。
    * 为了使用者，应该将数据域设为私有的（private），将访问器方法和修改器方法设为公有的（public）。
    * 为了扩展者，应该将数据域和方法都设为保护的（protected）。扩展者的合约包含使用者的合约。
  + 类应该使用private修饰符隐藏其数据，以免用户直接访问它。
- 使用静态修饰符static
  一个属性如果被类中的所有实例所共享，它应该声明为静态的。
- 使用继承或包容
  + 继承与包容之间的区别就是“是”（is-a）关系与“具有”（has-a）关系之间的区别。
  + 如果要求多态性，使用继承来设计。如果不需要关系多态性，包容设计会更好。因为对包容设计得类，类之间的依赖性要比使用继承设计的类弱一些。
- 使用接口或抽象类
  * 接口和抽象类都能用于描述一般性的公有特征。强是关系使用类来模拟，弱是关系使用接口来模拟。
  * 一个子类只能扩展一个父类，但是可以实现多个接口，所以接口比抽象类更灵活。
  * 要将接口和抽象类的好处结合起来，可以创建一个接口和一个实现该接口的抽象类，这样的抽象类称为`便利类(convenience class)`。

  ### 用顺序图和状态图模拟动态行为
  - UML图符仅仅用来描述类的属性和方法以及类之间的静态关系。
  - 顺序图：通过刻画方法调用的时间顺序来描述对象之间的交互作用。
    + **类角色（clss role）**表示对象所起的作用，顺序图顶部的对象表示类角色。
    + **生命线（lifeline）**表示在某段时间内对象是存在的，用对象引出的垂直虚线（点线）表示。
    + **激活（activation）**表示对象执行一项操作所用的时间段，使用位于生命线上的窄矩形表示。
    + **方法调用（method invocation）**表示对象之间的通信，用标有调用方法指令的水平箭头表示。
    [!顺序图示例]()
  - `状态图（statechart diagram）`用来描述对象的控制流。状态图包含下列元素：
    + 状态（state）表示对象在生存期内的状况，包括对象满足某些条件、执行某些操作或等待某些事件发生等。每个状态都有一个名字，用圆角矩形表示它们，初始状态例外，用实心小圆表示。
    + 转移（transition）表示两个状态之间的关系，表示一个对象执行某些操作后从一种状态转变为另一种状态。用标有合适方法调用的实线箭头来表示转移。
    [!状态图示例]()

### 使用Java API在构架基础上编程
- Java应用程序接口（Application Program Interface，API）由许多类和接口组成。
  java.lang包含Java核心类，每个Java程序隐含地导入该包。
  javax.swing包含用于开发Swing GUI程序的轻型图形用户界面组件。
  java.util包含许多工具。如StringTokenizer、Data、Calendar和GregorianCalendar等。

  ---

## 复习小结
- 接口是一种特殊的类，因此接口也可以用来声明类型。
```
Comparable tt = new Comparable() {
            @Override
            public int compareTo(Object o) {
                return 0;
            }
        };
```
  上述写法类似于内部类。

---

## 编程练习
- 习题10.1
	写成如下形式：
	```java
	long n = 1;
	        Rational sum = new Rational();
	        while (n < 100) {
	            Rational r = new Rational(n, n + 1);
	            sum = sum.add(r);
	            n++;
	        }
	```
  当n增加到23时，sum=2292289173/118982864,再进行运算时，会出现除零错误，原因是分子的值超出了Integer类的最大值2147483647。因此最终得不到类似`分子/分母`形式的结果。如果在每次循环结束后，将sum转换成double类型，就可以得到最后double类型的结果了：
  ```java
  long m = 1;
        double sum1 = 0;
        while (m < 100) {
            Rational r = new Rational(m, m + 1);
            sum1 += r.doubleValue();
            m++;
        }
  ```
- 其余习题10.3 10.4源代码见我的Github： [chapter10](https://github.com/whtis/Java-Exercises/tree/master/chapter10/src)






---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>