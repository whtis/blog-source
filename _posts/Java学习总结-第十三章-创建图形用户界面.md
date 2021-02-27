title: Java学习总结--第十三章 创建图形用户界面
date: 2016-02-15 22:32:39
categories: java
tags: [java,总结,原创]
---

** 本文总结源自《Java语言程序设计》原书第五版，作者为Y.daniel Liang，习题及编程练习均参照此书。**

---

## 主要内容
本章综合第十一、十二章的内容，介绍GUI编程中常用的组件。

### Swing GUI组件的公共特性
- Component类是所有用户界面组件和容器的根类，Jcomponent类是大多数Swing组件的根类。
- GUI组件都有处理字体、颜色、大小、工具提示文本和边框等属性的方法。
- 可以在Jcomponent类的任何对象上设置边框。
  + TitleBorder(String title) 创建标题边框
  + LineBorder(Color color,int width) 创建线型边框
- **容器类Container类是Swing GUI组件类的父类。**

### 按钮（JButton）
`按钮（button）`是一种点击时触发行为事件的组件。Swing提供常规按钮、开关按钮、复选框和单选按钮。
- Jbutton可以响应多种类型的事件，通常我们只需要响应ActionEvent事件。
- 按钮可以设置工具提示文本（`setToolTipText(xxx)`）和热键（`setMnemonic(xxx)`）。

#### 图标
- 图标是一个固定大小的图片，典型的图标体型较小，用于装饰组件。
```java
Icon icon = new ImageIcon("xxx");
```
**java目前支持GIF、PNG、JPEG三种图像格式。**
- 每个常规按钮有一个默认图标，一个按下图标和一个在上图标，使用如下方法进行设置：
  + setPressedIcon("XXX") 设置按钮按下的图标
  + setRolloverIcon("xxx") 设置鼠标停留在按钮上时的图标

#### 对齐方式
- `水平对齐（horizontal alignment）`指定以什么样的方式在**按钮**上放置文本和图标。
```java
setHorizontalAlignment(int)
```
  + int的取值为：SwingConstants.LEADING(LEFT、CENTER、RIGHT、TRAILING)。默认的是最后一种（右对齐）。
- `垂直对齐（vertical alignment）`指定以什么样的垂直方式在按钮上放置文本和图标。
```java
setVerticalAlignment(int)
```
  + int的取值为：SwingConstants.TOP(CENTER、BOTTOM)，默认的是CENTER。

#### 文本位置
- `水平文本位置（horizontal text position）`指定文本相对于图标的水平位置。
- `垂直文本位置（Vertical text position）`指定文本相对于图标的垂直位置。
- 设置方式和默认值与对齐方式相同。

### 复选框（JCheckBox）
- 一个`开关按钮（toggle button）`有两种状态，JToggleButton类继承AbstractButton并实现了一个开关按钮。开关按钮分为两种：
1、JCheckBox 复选按钮（方形）
2、 JRadionButton 单选按钮（圆形）
- JCheckBox先触发的是ItemEvent事件，然后触发ActionEvent事件。要确定复选框是否被选中，使用`isSelected()`方法。

### 单选按钮（JRadioButton）
- `单选按钮（radio button）`，或者叫`选择按钮(option button)`,让用户从一组选项中选择唯一的一个选项。
- 单选按钮使用Java.swing.ButtonGroup类的实例进行组织，并使用add方法添加按钮
```java
ButtonGroup group = new ButtonGroup();
group.add(xx1);
group.add(xx2);
```
如果没有创建按钮组，就无法达到单选的目的。
- JRadioButton先触发的是ItemEvent事件，然后触发ActionEvent事件。要确定复选框是否被选中，使用`isSelected()`方法。

### 标签（JLable）
- `标签（lable）`是显示一小段文字、一幅图片或二者皆有的区域。
- JLable继承自JComponent类，具有与JButton类似的属性。

### 文本域（JTextField）
- `文本域（text field）`可以用于输入或显示字符串。（单行）
- JTextField继承自JTextComponent类。
- JTextField触发的是ActionEvent事件（回车键触发）。

### 文本区（JTextArea）
- 文本区用户输入多行文本。
- 可以创建指定行列的文本区，JTextArea继承自JTextComponent类。
- JTextArea无法滚动，但可以创建一个`JScrollPane`对象处理滚动。
```java
JScrollPane scrollPane = new JScrollPane(new JTextArea());
```
- 常用属性
  + setLineWrap（boolean b） 是否换行
  + setWrapStyleWord（boolean b） 是否按单词换行，默认按照字符换行
  + setEditable（boolean b） 是否可编辑

### 组合框（JComboBox）
- `组合框（combo box）`，也叫`选择列表（choice list）`或`下拉式列表（drop-down list）`，它包含项目的一个列表，用户能够从中选择。**使用它可以限制用户的选择范围并能避免对输入数据有效性的繁复检查。**
- JComboBox可以引发ActionEvent和ItemEvent事件。选中一个新的项目是，JComboBox会产生两次ItemEvent事件，一次是取消前一个项目，另一次是选中当前项目。
- 常用方法
  + getSelectedItem（）方法返回已经选定的项目
  + getSelectedIndex（） 返回选中的项目次序号

### 列表框（JList）
- `列表框（list）`是一个组件，与JComboBox类似，但是它允许用户选择一个或多个项目。
- 选择模式属性selectionMode取值如下，默认为多区间选择
  + SINGLE_SELECTION 单项选择
  + SINGLE_INTERVAL_SETECTION 单区间选择（允许选择多项，但是必须连续）
  + MULTIPLE_INTERVAL_SETECTION 多区间选择
  ```java
  JList.setSelectionMode(ListSelectionModel.SINGLE_INTERVAL_SELECTION);
  ```
- JList不能自动滚动，可以参考文本区的方法设置滚动。
- JList触发ListSelectionEvent事件。
- 常用方法：
  + getSelectedIndices（） 以数组形式返回选中的次序号
  + getSelectedValues（） 以数组形式返回选中的值
  + getVisibleRowCount（） 返回可见行数

### 滚动条（JScrollbar）
- `滚动条（scrollbar）`是一个控制器，它使用户能从值得一个范围中进行选择。
- JScrolBar的属性如下：
  + orientation（方向）：指定滚动条的水平或垂直模式。
  + maximum（最大值）: 指滚动条的最大值。
  + minimum（最小值）
  + visibleAmount（宽度，也叫广度）：是指滚动块的相对宽度。滚动块在屏幕上的显示的实际宽度取决于最大值及visibleAmount的值。
  + value：表示滚动条当前的值。
  + blockIncrement（块增量）：是用户点击滚动条的块增加（减少）区时所增加（减少）的值。
  + unitIncrement（单位增量）：用户点击单位增加（减少）区所增加（减少）的值。
  + **滚动条上的宽度对应于`maximum+visibleAmount`。当滚动条设为最大值时，滚动块的左端在maximum处，右端在maximum+visibleAmount处。
- 用户改变滚动条的值时，滚动条产生AdjustmentEvent的一个实例。
- 可以使用构造方法或者使用`setOrientation`方法来指定滚动条的方向。默认情况下，属性maximum的值为100，minimum为0，blockIncrement为10，visibleAmount为10。

### 滑动块（JSlider）
- JSlider与JScrollBar类似，但是JSlider具有更多的属性和更多的显示形式。
- JSlider允许用户以图形方式在指定的区间中选择一个数值。滑动块可以在主标记以及次标记之间滑动。标记间的像素值是由`setMajorTickSpacing`和`setMinorTickSpacing`方法控制的。
- Slider可以带或不带标记，可以有或没有标签，可以水平显示或垂直显示。
- 垂直滚动条的值从上向下增加，但是垂直滑动块的值从上向下减少。
- 改变滑动块的值时，滑动块产生javax.swing.event.ChangeEvent的一个实例。

### 创建多个窗口
新开的窗口叫做`子窗口（subwindow）`，主框架叫`主窗口（main window）`。
- 从应用程序创建一个子窗口，需要创建JFrame的一个子类，用于定义任务和通知新窗口做什么。然后，在程序中创建该子类的一个实例，通过把它设为可见的即可弹出新窗口。

---

## 复习小结
- 可以在任何Swing组件使用边框，边框和图标都可以共享。
- AbstractButton类中的常量LEFT、RIGHT……也可以被许多其他Swing组件使用。由于所有的Swing组件都实现了SwingConstants，因此各种组件都可以通过SwingConstants引用这些常量。因此`JButton.CENTER和SwingConstants.CENTER`作用是一样的。

---

## 编程练习
- 习题13.1 13.4 13.5 13.8 13.10 13.14 13.15源代码见我的Github： [chapter13](https://github.com/whtis/Java-Exercises/tree/master/chapter13/src)

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>