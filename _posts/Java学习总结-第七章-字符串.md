title: Java学习总结-第七章 字符串
date: 2016-01-26 21:33:26
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
Java中，将字符串看作是对象，提供了String类、StringBuffer类和StringTokenizer类来存储和处理字符串。本章总结`字符串(string)`的处理。

### 字符串类String
- 字符串的`快捷初始化(shorthand initializer)`：
```java
String string = new String("xxx");
String message = "xxx";
```

- 永久字符串与规范字符串
 + 字符串是永久的。
 + 如果两个String对象是通过快捷初始化用相同的字符串直接量构造的，则Java虚拟机为了提高效率，将它们存储在同一对象中。这样的字符串称为`规范字符串（canonical string）`。
- 可以使用String对象的intern方法返回一个规范字符串，这种字符串与使用快捷初始化创建的字符串相同。
- String类的几个常用方法
  + charAt(int index) 提取指定字符
  + length() 返回数组的长度
  + concat（String s） 连接字符串s
  + subString(int beginIndex) 返回从beginIndex开始到结束的字符串
  + subString(int beginIndex，int endIndex) 返回从beginIndex开始到endIndex-1的字符串
  + regionMathches() 比较两个字符串的某一部分是否相等
- 字符串的比较
  + 使用"=="运算符检测两个字符串是否具有相同的引用。
  + 使用equals()方法对对象的内容进行比较。
  + 使用compareTo()方法比较两个字符串。返回值由字典序决定。
- 字符串的转换
  + toLowerCase() 将字符串转换成小写
  + toUpperCase() 将字符串转换成大写
  + trim() 删除字符串两端的空格
  + replace(oldChar,newChar) 用新的字符替换旧的字符
  + replaceAll(oldChar,newChar) 用新的字符替换全部旧的字符
- 获取字符串中的一个字符或子串
  + indexOf() 得到某个字符
  + lastIndexOf() 得到某个字符串
- 字符串与数组之间的转换
  + 字符串转换为数组： toCharArray() 方法
  + 数组转换为字符串： 使用构造方法String(char[])或者valueOf(char[])方法

### 字符类Character
- 常用的方法如下：
  + charValue() 返回该对象的char值
  + is*() 判断该字符是否是*

### 字符串缓冲类StringBuffer
- StringBuffer类比String类更加灵活，可以在字符缓冲区中添加、插入或追加新的内容。字符串一旦创建后，它的值就不再改变。
- 常用方法如下：
  + append() 在字符缓冲区中追加
  + deleteCharAt() 删除特定的字符
  + setCharAt() 在特定位置设置一个新字符
  + insert() 插入一个新内容
  + reverse() 倒置字符串缓冲区中字符的顺序
  + capacity() 返回字符串缓冲区的现有容量
  **String类的方法StringBuffer类同样拥有**
- 字符串的长度一定小于或等于缓冲区的容量。如果字符串的长度超过了缓冲区的容量，则缓冲区的容量将会变成2*（原缓冲区容量+1），因为在计算机内部，字符串缓冲区是一个字符数组。

### 字符串令牌类StringTokenizer
- 该类用来将字符串分解为片段，以便提取和处理字符串中的信息。
- 常用方法：
  + StringTokenizer(String s,String delim,boolean returnDelims) 
  使用指定的定界符构造字符串s的一个StringTokenizer对象。字符串delim中的每一个字符都是定界符。如果returndelims为true，那么定界符也看成令牌。
  + StringTokenizer(String s) 
  使用默认的定界符“\t \n \sr”（空格、制表符、换行符和回车符）构造字符串s的一个StringTokenizer对象，并且定界符不能算作令牌。
  + countTokens() 返回所包含的令牌数
  + hasMoreTokens() 如果该对象中还有令牌，返回true
  + nextToken() 返回下一个令牌

### 字符串扫描类Scanner
- StringTokenizer中，定界符是单个字符，使用Scanner类useDelimiter()方法，来指定一个单词作为定界符。
- Scanner类可以实现从键盘中读取输入。
```java
Scanner s = new Scanner(System.in);
```

### 命令行参数
- 使用如下命令行可以从命令行向main方法传递参数，参数保存在String[]类型的args数组中。
```java
java className arg0 arg1 arg2 ……
```
---

## 复习小结
- String类重写了equals()方法，因此可以用string.equals(string1)比较两个字符串；StringBuffer没有重写equals()方法，因此不能直接使用。
- equals()方法最初是用来比较地址是否相等的。
- StringTokenizer类没有无参构造方法。一个方法是否需要有无参构造方法取决于无参构造方法是否有意义，对StringTokenizer类来说，StringTokenizer对象必须是为某个字符串而创建的，因此不需要无参构造方法。
- 使用eclipse或者Intellij时，如果需要向main方法传递参数，乘号必须写成`" *"`或`"* "`才能正常显示结果。

---

## 编程练习
习题7.3 7.4 7.5 7.7 7.9 7.11 7.12 7.14 7.18源代码见我的Github： [chapter7](https://github.com/whtis/Java-Exercises/tree/master/chapter7/src)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>