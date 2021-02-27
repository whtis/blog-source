title: spring4学习笔记（三）--SpEL:Spring表达式语言
date: 2017-10-13 15:03:21
categories: java web
tags: [spring4,java]
---

## spEL:Spring 表达式语言

- 是一个支持运行时查询和操作对象图的强大的表达式语言。
- 语法类似于EL：SpEL使用`#{...}`作为定界符，所有在大框号中的字符都将被认为是`SpEL`
- SpEL为Bean的属性进行动态赋值提供了便利。
- 通过SpEL可以实现：
  + 通过 bean 的 `id` 对 bean 进行引用
  + 调用方法以及引用对象中的属性
  + 计算表达式的值
  + 正则表达式的匹配

### SpEL 可以表示基本的字面值

 ```xml
 <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
   <property name="user" value="#{2}"></property>
   <property name="password" value="#{3.14}"></property>
   <property name="driverClass" value="#{false}"></property>
   <property name="jdbcUrl" value="#{“whtis”}"></property>
 </bean>
 ```

### SpEL 引用 bean、属性和方法

- 引用其他对象：

```xml
<!-- 通过 value 属性和 SpEL 配置 bean 之间的应用关系 -->
<property name="prefix" value="#{prefixGenerator}"></property>
```

- 引用其他对象的属性：

```xml
<!-- 通过 value 属性和 SpEL 配置 bean 之间的应用关系 -->
<property name="suffix" value="#{sequenceGenerator2.suffix}"></property>
```

- 调用其他方法，还可以链式操作

```xml
<!-- 通过 value 属性和 SpEL 配置 suffix 属性值为另一个 bean 的方法的返回值 -->
<property name="suffix" value="#{sequenceGenerator2.toString()}"></property>
```

- 调用静态方法或静态属性：通过`T()`调用一个类的静态方法，它将返回一个 `Class Object`，然后再调用相应的方法或属性。

```xml
<property name="initValue" value=#{T(java.lang.Math).PI}></property>
```

### SpEL支持的运算符号

- 算数运算符：`+，-，×，/，%，^`
- 加号还可以用作字符串连接
- 比较运算符：`<，>，==，<=，>=，lt，gt，eq，le，ge`
- 逻辑运算符：`and，or，not，|`
- if-else运算符：`?:(temary),?:(Elvis)`
- if-else的变体

```xml
<constructor-arg value="#{kenny.song:'Greensleeves'}"
```

- 正则表达式：`matches`



---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
