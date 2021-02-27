title: spring4 学习笔记（一）--spring 简介
date: 2017-10-06 19:19:15
categories: java web
tags: [spring4,java]
---

### spring4 HelloWorld

- spring 是一个IOC（DI）和AOP容器框架
- 具体描述
  + 轻量级：**Spring是非侵入式的**-基于Spring开发的应用中的对象可以不依赖`Spring`的`API`
  + 依赖注入
  + 面向切面编程（AOP）
  + 容器
  + 框架
  + 一站式

### IOC和DI

- IOC（Inversion of Control）：其思想是**反转资源获取的方向**。传统的资源查找方式要求组件向容器发起请求查找资源。作为回应，容器适时的返回资源。而应用了IOC以后，则是**容器主动地将资源推送给它所管理的组件，组件所要做的仅是选择一种合适的方式来接受资源**。这种行为也成为查找的被动形式。
- DI（Dependency Injection）：IOC的另一种表述方式：即：**组件以一些预先定义好的方式（例如：setter方法）接受来自如容器的资源注入**。相对于IOC来说，这种表述更直接。
 - IOC
   + 分离接口与实现
   + 工厂模式
   + 采用反转控制

### Spring容器

- 在Spring IOC 容器读取Bean配置创建Bean实例之前，必须对它进行实例化。只有在容器实例化后，才可以从IOC容器里获取Bean实例并使用。
- Spring提供了两种类型的IOC容器实现
  + BeanFactory： IOC容器的基本实现
  + ApplicationContext：提供了更多的高级特性，是BeanFactory的子接口。
  + BeanFactory是Spring框架的基础设施，面向Spring本身；ApplicationContext面向使用Spring框架的开发者，几乎所有的应用场合都直接使用ApplicationContext而非底层的BeanFactory
  + 无论使用何种方式，配置文件是相同的

#### ApplicationContext的主要实现类

- ClassPathXmlApplicationContext:从类路径下加载配置文件
- FileSystemXmlApplicationContext： 从文件系统中加载配置文件
- ConfigurableApplicationContext扩展于ApplicationContext，新增加两个主要方法：refresh（）和close（），让ApplicationContext具有启动、刷新和关闭上下文的能力。
- ApplicationContext在初始化上下文的时候就实例化所有单例的Bean。（Bean的作用域）
- WebApplicationContext是专门为WEB应用而准备的，它允许从相对于WEB根目录的路径中完成初始化工作。

### 从 IOC 容器中获取 Bean 实例

- BeanFactory 中的方法，常用的有两种：
  + 用id定位到 IOC 容器中的 Bean
  + 利用类型返回　IOC 容器中的 Bean。但要求 IOC 容器中必须只能有一个该类型的bean


---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
