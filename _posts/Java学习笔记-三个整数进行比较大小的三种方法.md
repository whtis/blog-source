title: Java学习笔记--三个整数进行比较大小的三种方法
date: 2015-11-13 20:43:59
categories: java
tags: [java,原创,java学习笔记]
---
- 对abc三个整数从小到大进行排序，有以下几种方法：

* 方法一：首先对a和b进行比较排序，使得`a<=b`,再对b和c排序，使得`b<=c`，最后再对a和b进行比较，就可以得到正确的顺序了。代码如下：

```
 public class $3_2 {
    public static void main(String[] args) {
        int a = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int b = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int c = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int tmp;
        if (a >= b) {
            tmp = b;
            b = a;
            a = tmp;
        }
        if (b >= c) {
            tmp = c;
            c = b;
            b = tmp;
        }
        if (a >= b) {
            tmp = b;
            b = a;
            a = tmp;
        }
        System.out.println("the order is " + a + " " + b + " " + c);

    }
}
```

* 方法二：使用嵌套的if循环，讨论当`a>=b`以及`a<b`的情况。代码如下：

```
 public class $3_2 {
    public static void main(String[] args) {
        int a = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int b = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int c = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int tmp;
        if (a >= b) {
            if (b >= c) {
                //c<=b<=a
                tmp = a;
                a = c;
                c = tmp;
            } else {
                if (a >= c) {
                    //b<=c<=a
                    tmp = a;
                    a = b;
                    b = c;
                    c = tmp;
                } else {
                    //b<=a<=c
                    tmp = b;
                    b = a;
                    a = tmp;
                }
            }
        } else {
            if (b >= c) {
                if (a <= c) {
                    //a<=c<=b
                    tmp = c;
                    c = b;
                    b = tmp;
                } else {
                    //c<=a<=b
                    tmp = b;
                    b = a;
                    a = c;
                    c = tmp;
                }
            }
        }
        System.out.println("the order is " + a + " " + b + " " + c);

    }
}
```

* 方法三：使用三个boolean类型的值表示`a>=b`、`b>=c`、`a>=c`,并分别进行讨论。八种情况中有两种不成立，故舍去。代码如下：

```
 public class $3_2 {
    public static void main(String[] args) {
        int a = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int b = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int c = Integer.parseInt(JOptionPane.showInputDialog(null, "请输入一个整数："));
        int tmp;
        boolean b1 = a >= b;
        boolean b2 = a >= c;
        boolean b3 = b >= c;
         if (b1 && b2 && b3) {//b3多余 c<=b<=a
            tmp = a;
            a = c;
            c = tmp;
        }
        if (b1 && b2 && !b3) { //b<=c<=a
            tmp = a;
            a = b;
            b = c;
            c = tmp;
        }
        if (b1 && !b2 && !b3) { //b<=a<=c
            tmp = b;
            b = a;
            a = tmp;
        }
        if (!b1 && b2 && b3) { //c<=a<=b
            tmp = c;
            c = b;
            b = a;
            a = tmp;
        }
        if (!b1 && !b2 && b3) { //a<=c<=b
            tmp = b;
            b = c;
            c = tmp;
        }
        if (!b1 && !b2 && !b3) { //a<=b<=c

        }
        System.out.println("the order is " + a + " " + b + " " + c);
    }
}
```

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>