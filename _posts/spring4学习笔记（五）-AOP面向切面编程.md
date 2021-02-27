title: spring4学习笔记（五）--AOP面向切面编程
date: 2017-11-04 22:14:48
categories: java web
tags: [spring4,java]
---

## AOP 简介
- AOP(Aspect-Oriented Programming,面向切面编程)：是对传统的`OOP`(Object-Oriented Programming 面向对象编程)的补充。
- AOP 的主要编程对象是`切面(aspect)`，和`切面模块化横切关注点`。
- 在应用AOP编程时，仍然需要定义公共功能，但可以明确的定义这个功能在哪里，以什么方式应用，并且不必修改受影响的类。这样一来横切关注点就被模块化到特殊对象(切面)里。
- AOP的好处：
  + 每个事物逻辑位于一个位置，代码不分散，便于维护和升级
  + 业务模块更简洁，只包含核心业务代码
- AOP 很适合做日志、参数验证等工作。

## AOP 术语
- 切面(Aspect): 横切关注点(跨越应用程序多个模块的功能)被模块化的特殊对象
- 通知(Advice): 切面必须要完成的工作
- 目标(Target): 被通知的对象
- 代理(Proxy): 向目标对象应用通知之后创建的对象
- 连接点（Joinpoint）：程序执行的某个特定位置：如类某个方法调用前、调用后、方法抛出异常后等。连接点由两个信息确定：方法表示的程序执行点；相对点表示的方位。
- 切点（pointcut）：每个类都拥有多个连接点：一个类的所有方法实际上都是连接点，即连接点是程序类中客观存在的事务。AOP 通过切点定位到特定的连接点。类比：连接点相当于数据库中的记录，切点相当于查询条件。切点和连接点不是一对一的关系，一个切点匹配多个连接点，切点通过 `org.springframework.aop.Pointcut`接口进行描述，它使用类和方法作为连接点的查询条件。

## Spring AOP

### AspectJ
在 Spring2.0以上版本中， 可以使用基于AspectJ注解或基于XML配置的AOP

#### 在Spring中启用AspectJ注解支持
- 要在Spring应用中使用AspectJ注解， 必须在classpath下包含AspectJ类库: aopalliance.jar、aspectj.weaver.jar 和 spring-aspects.jar
- 要在Spring IOC容器中启用AspectJ注解支持， 只要在Bean配置文件中定义一个空的XML元素 <aop:aspectj-autoproxy>
当 Spring IOC容器侦测到Bean配置文件中的 <aop:aspectj-autoproxy> 元素时， 会自动为与AspectJ切面匹配的Bean创建代理.

#### 用AspectJ注解声明切面
- 要在Spring中声明AspectJ切面， 只需要在IOC容器中将切面声明为Bean实例。 当在Spring IOC容器中初始化AspectJ切面之后， Spring IOC容器就会为那些与AspectJ切面相匹配的 Bean 创建代理。
- 在 AspectJ 注解中， 切面只是一个带有`@Aspect` 注解的Java类。
- 通知是标注有某种注解的简单的 Java 方法。
- AspectJ 支持 5 种类型的通知注解:
  + `@Before`: 前置通知， 在方法执行之前执行
  + `@After`: 后置通知， 在方法执行之后执行
  + `@AfterRunning`: 返回通知， 在方法返回结果之后执行
  + `@AfterThrowing`: 异常通知， 在方法抛出异常之后
  + `@Around`: 环绕通知， 围绕着方法执行
- 切入点表达式的写法一般是`execution(...)`，我们可以通过声明一个标有`#Pointcut(exection(...))`的空方法，来对切入点表达式进行**合并**。

```java
@Pointcut("exection(...) || exection(...)")
private void loggingPoeration(){}
```
  + 切入点方法的访问控制符同时也控制着这个切入点的可见性。如果切入点要在多个切面中共用，最好将它们集中在一个公共的类中。在这种情况下，它们必须被声明为`public`。在引入这个切入点时，必须将类名也包括在内。如果类没有与这个切面放在同一个包中，还必须包含包名。
  + 其他通知可以通过方法名称引入该切入点。
- 为了让通知访问当前连接点的细节，我们可以在通知方法中声明一个类型为`JoinPoint`的参数，从而访问到方法名称、参数值、异常通知这些东西。

```java
private void loggingPoeration(JointPoint jointPoint){
    String methodName = jointPoint.getSignature().getName();
    System.out.println(Arrays.toString(jointPoint.getArgs()));
}
```

- 一个切点可以包含一个或多个通知。
- 在同一个连接点上应用不止一个切面时，除非明确指定，否则它们的优先级是不确定的。
  + 切面的优先级可以通过实现`Ordered`接口或利用`@Order`注解指定。
  + 使用`@Orader`注解，序号越小，优先级越高。

#### 用基于XML配置文件声明切面
- 正常情况下，基于注解的声明要优先于基于XML的声明。通过AspectJ注解，切面可以与AspectJ兼容，而基于XML的配置则是Spring专有的。由于 AspectJ得到越来越多的AOP框架支持，所以以注解风格编写的切面将会有更多重用的机会。
- 一个完整的`AspectJ`基于xml文件的配置如下：

```xml
<aop:config>
    <aop:pointcut id="" expression="execution(xxx)"/>
    <aop:aspect id="" ref="" order="">
        <aop:before method="xxx" pointcut-ref=""/>
        <aop:after method="" pointcut-ref=""/>
    </aop:aspect>
</aop:config>
```

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
