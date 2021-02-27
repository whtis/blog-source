title: GUI小练习-简易计算器
date: 2016-02-16 18:51:30
categories: java
tags: [java,原创,小练习]
---

## 简易说明

- 学习了Java图形界面设计，写了一个简易的计算器，实现的功能如下：
  + 基本的加减乘除四则运算
  + 连续计算
  + 运算结果记忆
  + 程序启动次数统计
- 本来打算添加其它功能，不过基本框架出来也就不打算在这部分停留太久，暂时写下设计的功能，等以后有更简单的方法处理连续计算的时候再回来重写一下吧：
  + 显示结果的处理，包括两个整数作运算，结果应该显示为整数、浮点数显示的位数设定等。目前为了方便，统一转为了浮点数进行计算，显示的结果也是浮点数。
  + 初始设计了四个功能键，依次为`Ac`（清零）、`Cv`（小数和分数的转化）、`Re`（最近一次结果记忆），`←`（删除），目前除`Cv`外都已实现。
  + 目前显示结果为单行显示，最初考虑的是双行，但因为文本对齐和设置的问题无法处理，最终采用的JTextField进行单行显示。
  + 组件的等比例放大和缩小问题。目前只能做到自动，因此将其设置成为了不可更改大小,避免破坏布局。

## 遇到的问题及解决方案记录

### 布局问题
- 如何拼凑无缝的界面
  + 解决方法:略

### 加减乘除运算问题
- 第一次就按运算符的处理
  + 如果是初始操作，无视之
  + 判断StringBuffer对象里是否有东西，如果没有，直接设置运算符compute
- 多次按运算符的处理
  + 创建一个string类型的值来存储运算符，但同时只能设置一个
- 连续运算的问题（貌似链表处理比较简单？没学过，布吉岛...）
  + 计算器能够正常运行的一系列设计，包括stringbuffer、compute、result等
  ```java
  private void computeAction(ActionEvent e, JTextField textField) {
  //
  }
  ```

### 小数点的问题
- 直接按小数点应该将其设置成正确的小数形式
```java
if (stringBuffer.toString().startsWith(".")) {
            stringBuffer.insert(0, "0");
        }
```
- 小数点在一次运算中只能出现一次，多按无效
  ```java
  case ".":
        boolean hasDot = false;
        for (int i = 0; i < stringBuffer.length(); i++) {
            if (stringBuffer.charAt(i) == '.') {
                hasDot = true;
            }
        }
        if (!hasDot) {
            capicaty++;
            stringBuffer.append(".");
            textField.setText(stringBuffer.toString());
        }
        break;
   ```

### 等于问题
- 仅仅在初始时输入数字，不进行任何运算，按下等于号应该显示初始输入的数字，并将result设置为相关的值，从而进行下次运算
```java
if(capicaty<=0) {
	textField.setText(String.valueOf(result));
    reResult = result;
    compute = null;
}
```

### 除法问题
- 出现不合法的运算会得到Infinity，为了界面友好，显示为'0'
```java
case "÷":
    result /= Double.parseDouble(stringBuffer.toString());
    if (String.valueOf(result).equals("Infinity")) {
        result = 0;
    }
    break;
```

## 代码
- 完整的代码见我的Github：[Calculator](https://github.com/whtis/Calculator)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>