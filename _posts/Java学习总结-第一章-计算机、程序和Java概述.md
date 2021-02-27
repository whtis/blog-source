title: Java学习总结--第一章 计算机、程序和Java概述
date: 2015-12-20 20:42:57
categories: java
tags: [java,总结,原创]

---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
本章简单介绍了计算机和Java有关的知识，让人有初步了解。

### 二进制与十进制数的转换
给定二进制数<img src="http://latex.codecogs.com/gif.latex?b_{n}b_{n-1}b_{n-2}...b_{2}b_{1}b_{0}" />,与它相等的十进制数为
<img src="http://latex.codecogs.com/gif.latex?b_{n}*2^{n}&plus;b_{n-1}*2^{n-1}&plus;b_{n-2}*2^{n-2}&plus;...&plus;b_{2}*2^{2}&plus;b_{1}*2^{1}&plus;b_{0}*2^{0}" />
把一个十进制数d转换为二进制数，就是求满足
<img src="http://latex.codecogs.com/gif.latex?d=b_{n}*2^{n}&plus;b_{n-1}*2^{n-1}&plus;b_{n-2}*2^{n-2}&plus;...&plus;b_{2}*2^{2}&plus;b_{1}*2^{1}&plus;b_{0}*2^{0}" />
的位<img src="http://latex.codecogs.com/gif.latex?b_{n},b_{n-1},b_{n-2},...,b_{2},b_{1},b_{0}" />。用2不断的除d，直到商为0为止，余数即为所求的位<img src="http://latex.codecogs.com/gif.latex?b_{0},b_{1},b_{2},...,b_{n-2},b_{n-1},b_{n}" />。

### 十六进制与十进制数的转换
给定十六进制数<img src="http://latex.codecogs.com/gif.latex?h_{n}h_{n-1}h_{n-2}...h_{2}h_{1}h_{0}" />,与它相等的十进制数为
<img src="http://latex.codecogs.com/gif.latex?h_{n}*16^{n}&plus;h_{n-1}*16^{n-1}&plus;h_{n-2}*16^{n-2}&plus;...&plus;h_{2}*16^{2}&plus;h_{1}*16^{1}&plus;h_{0}*16^{0}" />
把一个十进制数d转换为二进制数，就是求满足
<img src="http://latex.codecogs.com/gif.latex?d=h_{n}*16^{n}&plus;h_{n-1}*16^{n-1}&plus;h_{n-2}*16^{n-2}&plus;...&plus;h_{2}*16^{2}&plus;h_{1}*16^{1}&plus;h_{0}*16^{0}" />
的位<img src="http://latex.codecogs.com/gif.latex?h_{n},h_{n-1},h_{n-2},...,h_{2},h_{1},h_{0}" />。用16不断的除d，直到商为0为止，余数即为所求的位<img src="http://latex.codecogs.com/gif.latex?h_{0},h_{1},h_{2},...,h_{n-2},h_{n-1},h_{n}" />。

### 二进制数与十六进制数的转换
要把一个十六进制数转换为二进制数，只需要使用下表，把十六进制数的每一位转换为四位二进制数即可。

|    十六进制    |     二进制     |     十进制     |
| ------------: |--------------:| -------------:|
|    0          |    0          |    0          |
|    1          |    1          |    1          |
|    2          |    10         |    2          |
|    3          |    11         |    3          |
|    4          |    100        |    4          |
|    5          |    101        |    5          |
|    6          |    110        |    6          |
|    7          |    111        |    7          |
|    8          |    1000       |    8          |
|    9          |    1001       |    9          |
|    A          |    1010       |   10          |
|    B          |    1011       |   11          |
|    C          |    1100       |   12          |
|    D          |    1101       |   13          |
|    E          |    1110       |   14          |
|    F          |    1111       |   15          |

### Java的特点
**简单的(simple)、面向对象的(object-oriented)、分布式的(distributed)、解释性的(interpreted)、健壮的(robust)、安全的(secure)、结构中立的(architecture-neutral)、可移植的(portable)、高效的(high-performance)、多线程的(multithreaded)、动态的(dynamic)。**

### Java的编译和运行
- 编译：cmd下使用javac命令进行编译。
- 运行：cmd下使用java命令运行。
 
在使用cmd编译运行java程序时，需要注意如下代码：
```java
package test;
public class test {
	public static void main(String[] args) {
    	System.out.println("helloWorld");
   }
}
```
如果程序中含有包名，使用`java test`运行时会出现**找不到或无法加载主类 test**，解决这个问题有如下几种方法：
+ 方法一： 在编译路径下新建名为test的文件夹，然后将test.class文件拖入该文件夹，在编译路径下使用
```
java test.test
```
运行程序。
比如路径是 `C:\Users\ht\Desktop>` ,在此路径下我使用了 `javac test.java`进行了编译，那么这路径就是编译路径，为了成功运行上面的程序，我需要新建一个test文件夹，其路径为`C:\Users\ht\Desktop\test`，再将test.class文件拖到test文件夹内，然后在`C:\Users\ht\Desktop>`下使用 `java test.test`运行。
+ 方法二： 使用 `javac -d<目录>` 方式自动创建class文件路径。同样是上面的例子，在`C:\Users\ht\Desktop>`路径下，使用
```java
javac -d \Users\ht\Desktop test.java
```
编译，然后使用
```
java test.test
```
运行即可。
+ 方法三： 使用集成开发环境即可避免这类问题。这里推荐[Intellij IDEA](http://www.jetbrains.com/idea/)。
**建议初学者先使用cmd进行java的学习，这样可以理解一些基础性的东西，比如上面的问题，其实就是java的classpath的问题，如果懂java编译时如何对class文件定位，解决上述问题就很简单了。**

---

## 复习小结
- 第一章是对Java这门语言的概述，需要了解关键字、类和方法的命名方式、Java程序的编译运行等东西。
- Java提供了专门的Math类来处理进制转换。

---

## 编程练习
（略）


---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>