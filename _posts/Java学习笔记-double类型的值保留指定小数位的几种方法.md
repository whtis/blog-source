title: Java学习笔记--double类型的值保留指定小数位的几种方法
date: 2015-11-13 12:56:43
categories: java
tags: [java,原创,java学习笔记]
---
在java语言中，double类型的变量使用64bit存储空间进行存储。两个double类型的变量进行除法运算时，如果小数位过多，就需要进行保留位数操作。保留位数操作分为`四舍五入`型和`非四舍五入`型两种。本文以保留两位数为例：

- 四舍五入型
使用java.text类中的`DecimalFormat（）`方法可以对double类型的值进行保留位数操作。代码如下：

```
public class test {
    public static void main(String[] args) {
        double x = 1;
        double y =3;
        double z = x / y;
        DecimalFormat decimalFormat = new DecimalFormat("####.##");
        String str = decimalFormat.format(z);
        System.out.println(str);
    }
}
```

** `DecimalFormat`的`serRoundMode（）`方法，可以智能的设置小数位的处理方法，详见[API](http://docs.oracle.com/javase/7/docs/api/java/math/RoundingMode.html) **

- 非四舍五入型
  * 方法一：使用String类的`valueOf()`方法将double类型的值转变成String类型，之后使用`indexOf()`方法，确定小数点的位置，取此位置之后两位的substring即可。代码如下：

```
  public class test {
    public static void main(String[] args) {
        double x = 1;
        double y =3;
        double z = x / y;
        String str = String.valueOf(z);
        int situation = str.indexOf(".");   //查找小数点的位置并将其值赋给situation
        str = str.substring(0, situation + 3);
       System.out.println(str);
    }
}
```

  * 方法二：先使double类型的值乘以100，之后转变成int类型再除以100.00，得到包含两位小数的double值。代码如下：

```
public class test {
    public static void main(String[] args) {
        double x = 1;
        double y =3;
        double z = x / y;
        z = (int) (z * 100)/100.00;
       System.out.println(z);
    }
}
```


---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>