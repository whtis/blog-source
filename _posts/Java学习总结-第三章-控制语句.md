title: Java学习总结--第三章 控制语句
date: 2016-01-10 21:49:40
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
本章介绍了java语言中循环（loop）的使用方式。

### 条件语句
- if语句
- if...else语句
- 嵌套if语句
- switch语句
- 条件表达式：符号？和：在条件表示式中同时出现，构成java中唯一的三目运算符
  例如 `(x>y)?x:y`

### 循环语句
- while循环
- do-while循环
- for循环
**如果知道循环次数，就选择for循环；如果不知道次数，就选择while循环。**

### 关键字break和continue
- break: 跳出离它最近的循环体。
- continue： 跳出当次循环。
- 利用语句标号终止循环：
```java
outer:
  for(int i=0;i<0;i++){
  inner:
    for(int j=1;j<10;j++) {
        if (i*j>50) {
            break outer;
        }
        System.out.println(i*j);
    }   
  }
```
---

## 复习小结
- while循环执行最多`n`次，do-while循环执行次数最多是`n+1`次。
- 从jdk1.7开始，switch变量增至6个：`byte`,`int`,`long`,`short`,`enum`,`String`。后两个为新增的。
- 使用switch语句，可以处理多种情况，简化代码。
- for语句可以写为如下形式：
```java
for( ; ; ) {
    //do something
}
```
这样可以在不知道具体循环次数的情况下保证代码的正常运行。

---

## 编程练习
3.2 （三个整数排序）：见[三个整数进行比较大小的三种方法](http://www.whtis.com/2015/11/13/Java%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-%E4%B8%89%E4%B8%AA%E6%95%B4%E6%95%B0%E8%BF%9B%E8%A1%8C%E6%AF%94%E8%BE%83%E5%A4%A7%E5%B0%8F%E7%9A%84%E4%B8%89%E7%A7%8D%E6%96%B9%E6%B3%95/)

3.14 （查找两个最高分）编写程序，提示用户输入学生的数量及每个学生的名字和得分，而后显示最高分的学生。
解题思路：使用hashMap存入key-value，排序后显示，代码如下：(本题解法，若按照书中顺序，是无法完成的。看书时没有细想，参考了网上的解法。有时间再回来更新吧0.0)
```java
public class $3_14 {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<String, Integer>();
        Scanner scanner = new Scanner(System.in);
        System.out.println( "请输入学生的数量：");
        int num = scanner.nextInt();
        for (int i = 0; i < num; i++) {
            System.out.println("请输入学生的姓名： ");
            String name = scanner.next();
            System.out.println("请输入该生的成绩： ");
            int hh = scanner.nextInt();
            map.put(name, hh);
        }

        List<Map.Entry<String, Integer>> infolds = new ArrayList<Map.Entry<String, Integer>>(map.entrySet());

        //排序
        Collections.sort(infolds, new Comparator<Map.Entry<String, Integer>>() {
            public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
                return (o2.getValue() - o1.getValue());
//                return (o1.getKey()).toString().compareTo(o2.getKey());
            }
        });

        for (int i = 0; i < infolds.size(); i++) {
            String id = infolds.get(i).toString();
            System.out.println(id);
        }
    }
}
```

习题3.5 3.7 3.10 3.12 3.13 3.19 3.20 3.21 3.25 3.27 3.28 3.30 3.31 3.34源代码见我的Github： [chapter3](https://github.com/whtis/Java-Exercises/tree/master/chapter3/src)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>