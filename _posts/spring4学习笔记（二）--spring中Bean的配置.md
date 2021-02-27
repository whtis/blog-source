title: spring4学习笔记（二）--spring中Bean的配置
date: 2017-10-13 11:54:06
categories: java web
tags: [spring4,java]
---

# 配置形式

## 基于xml文件的方式

### Bean的配置方式

#### 通过全类名（反射）

```
<!--
  通过全类名配置bean
  class：bean 的全类名，通过反射的方式在 IOC 容器中创建 Bean。所以要求 Bean 中必须有无参数的构造器。
  id： 表示容器中的Bean，唯一。
  <Bean id="" class=""></Bean>
 -->
```

#### 依赖注入的方式

##### 构造器注入

- 通过构造方法注入 Bean 的属性值或依赖的对象，它保证了 Bean 实例在实例化后就可以使用。
- 构造器注入在` <constructor-arg>`元素里声明属性，该元素没有`name`属性

```xml
<!-- 通过构造方法来配置 Bean 属性 -->
<bean id="" class="" >
  <!-- 使用构造器注入属性值可以指定参数的位置和参数的类型！ 以区分重载的构造器！-->
  <constructor-arg value="" (type="" or index="") ></constructor-arg>
  <constructor-arg>
    <!-- 字面值注入，基本数据类型及其封装类、String等类型都可以采取字面值注入的方式 -->
    <value>250</value>
    <!-- 如果字面值包含特殊字符可以使用<![CDATA[]]>包裹起来 -->
    <><![CDATA[<beijing>]]><>
  </constructor-arg>
</bean>
```

##### 属性注入

- 属性注入即通过`setter`方法注入Bean的属性值或依赖的对象
- 属性注入使用 `<property> `元素，使用`name`属性指定 Bean 的属性名称，`value` 属性或` <value>` 子节点指定属性值。
- 属性注入是实际应用中最常用的注入方式。

```xml
<!-- 通过全类名的方式来配置 bean -->
<bean id="" class="" >
  <property name="" value="" ></property>
</bean>
```

###### 引用其他 Bean

- 在 Bean 的配置文件中，可以通过`<ref>`元素或者`ref`属性为Bean的属性或构造器参数指定对Bean的引用

```xml
<bean id="" class="">
  <property name="" value="" ref=""></property>
  <!--
    <property name="" value="" >
      <ref bean=""/>
    </property>
   -->
</bean>
```

- 也可以在属性或构造器里包含Bean的声明，这样的Bean称为内部Bean

```xml
<bean id="" class="">
  <property name="" >
  <!-- 内部 Bean，不能被外部引用，只能内部使用 -->
    <bean class="" >
      <constructor-arg value="" ></constructor-arg>
    </bean>
  </property>  
</bean>
//构造器注入类似
```

###### 注入参数详解：null和级联属性

- 赋`null`值

```xml
<constructor-arg><null/></constructor-arg>
```

- 级联属性

```xml
<!-- 为级联属性赋值。注意：属性（car）需要先初始化才能为级联属性赋值，否则会出错 -->
<property name="car.maxPrice" value=""></property>

```

###### 集合属性

- 在Spring中可以通过一组内置的xml标签（`<list>、<set>、<map>`）来配置集合属性。

```xml
<bean id="" class="" >
  <property name="list">
  <!-- 使用list为list类型的成员变量-->
    <list value="">
      <ref bean=""/>
    </list>
  </property>
</bean>

//set类似

<!-- 配置map属性值 -->
<bean id="" class="" >
  <property name="map">
  <!-- 使用map节点及 map 的 entry 子节点配置 Map类型的成员变量 -->
    <map>
      <entry key="" value=""></entry>
    </map>
  </property>
</bean>
```

- 使用`<props>`定义`java.util.Properties`，该标签使用多个`<prop>`作为子标签，每个`<prop>`标签必须定义`key`属性。

```xml
<!-- 配置properties属性值 -->
<bean id="" class="" >
  <property name="properties">
  <!-- 使用props节点及 props 的 prop 子节点配置 props 类型的成员变量 -->
    <props>
      <prop key="user">root</prop>
      <prop key="password">123456</prop>
      <prop key="jdbcUrl">jdbc:mysql:///test</prop>
      <prop key="driver">com.mysql.jdbc.Driver</prop>
    </props>
  </property>
</bean>
```

- 使用`utility scheme`定义集合

```xml
<!-- 配置独立的集合 bean，以供多个 bean 使用，需要引入util命名空间 -->
<util:list id="list">
    <ref bean=""/>
</util:list>
```

- 使用`p`命名空间

```xml
<!-- 通过p命名空间为 bean 的属性赋值，需要先导入 p 命名空间，相对于传统的配置方法，较简捷 -->
<bean id="" class="" p:xxx="" p:zzz-ref=""></bean>
```

###### 使用外部属性文件

- 在配置文件里配置 Bean 时，有时需要在Bean的配置里混入**系统部署的细节信息**（例如：文件路径，数据源配置信息等）。而这些部署细节实际上需要和Bean配置分离。
- Spring提供了一个`PropertyPlaceholderConfigurer`的`BeanFactory后置处理器`，这个处理器允许用户将Bean配置的部分内容外移到`属性文件`中。可以在Bean配置文件里使用形式为`${var}`的变量，`PropertyPlaceholderConfigurer`从属性文件里加载属性，并使用这些属性来替换变量。
- Spring还允许在属性文件中使用`${propName}`，以实现属性之间的相互引用。

 ```xml
<!-- 导入属性文件 -->
<context:property-placeholder location="classpath"db.properties"/>

<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
  <property name="user" value="${user}"></property>
  <property name="password" value="${password}"></property>
  <property name="driverClass" value="${driverClass}"></property>
  <property name="jdbcUrl" value="${jdbcUrl}"></property>
</bean>
```

##### 工厂方法注入（不常用）

#### 通过工厂方法（静态工厂方法&实例工厂方法）

##### 通过调用静态工厂方法创建Bean
- 调用静态工厂方法创建Bean是将`对象创建的过程封装到静态方法中`。当客户端需要对象时，只需要简单地调用静态方法，而不关心创建对象的细节。
- 要声明通过静态方法创建的Bean，需要在Bean的`class`属性里指定拥有该工厂的方法的类，同时在`factory-method`属性里指定工厂方法的名称。最后，使用`<constructor-arg>`元素为该方法传递方法参数。

```xml
<!-- 通过静态工厂方法来配置 Bean. 注意不是配置静态工厂方法实例，而是配置 bean 实例 -->
<!--
    class 属性： 指向静态方法的全类名
    factory-method: 指向静态工厂方法的名字
    constructor-arg：如果工厂方法需要传入参数，则使用constructor-arg 来配置参数
 -->
<bean id="" class="StaticCarFactory" factory-method="getCar">
  <constructor-arg value="xxx"></constructor-arg>
</bean>
```

相应的静态类如下：

```java
public class StaticCarFactory {

    static Map<String, Car> carMap = new HashMap();

    static {
        carMap.put("Audi", new Car("Audi","300000"));
        carMap.put("Ford", new Car("Ford", "400000"));
    }

    public static Car getCar(String name) {
        return carMap.get(name);
    }
}
```

##### 通过调用实例工厂方法创建Bean
- 实例工厂方法：`将对象的创建过程封装到另外一个对象实例的方法里`。当客户端需要请求对象时，只需要简单的调用该实例方法而不需要关心对象的创建细节。
- 要声明通过实例实例工厂方法创建的Bean
  + 在bean的`factory-bean`属性里指定拥有该工厂方法的Bean
  + 在`factory-method`属性里指定该工厂方法的名称
  + 使用`constructor-arg`元素为工厂方法传递方法参数

```xml
<!-- 配置工厂的实例 -->
<bean id="carFactory" class="InstanceCarFactory"></bean>
<!-- 通过实例工厂方法来配置 bean -->
<!--
    class 属性： 指向实例方法的 bean
    factory-method: 指向实例工厂方法的名字
    constructor-arg：如果工厂方法需要传入参数，则使用constructor-arg 来配置参数
 -->
<bean id="" factory-bean="carFactory" factory-method="getCar">
    <constructor-arg value="Ford"></constructor-arg>
</bean>
```

相应的实例类如下：

```java
public class InstanceCarFactory {

    private Map<String, Car> map = null;

    public InstanceCarFactory() {
        map = new HashMap<String, Car>();
        map.put("Audi", new Car("Audi", "300000"));
        map.put("Ford", new Car("Ford", "400000"));

    }

    public Car getCar(String name) {
        return map.get(name);
    }
}
```

#### FactoryBean




#### Spring bean自动装配（装配引用）

- spring IOC容器可以自动装配bean，需要做的仅仅是在`<bean>`的autowire属性里指定自动装配的模式。
  + `byType`：根据bean的类型和当前bean的属性的类型尽心自动装配。若 IOC 容器中有1个以上的类型匹配的bean，则抛异常。
  + `byName`：根据bean的名字和当前bean的setter风格的属性名进行自动装配，若有匹配的，则进行自动装配，若没有匹配的，则不装配
  + `constructor`：不推荐使用
    ```xml
    <bean id="" class="" autowire="byName"></bean>
    ```

- 在Bean配置文件里设置`autowire`属性进行自动装配将会装配Bean的所有属性。然而，若只希望装配个别属性时，`autowire`就不够灵活了。
- `autowire`属性只能选择一种进行配置
- 一般情况下不建议使用自动装配功能

#### bean 之间的关系：继承；依赖

##### bean 的继承

- Spring允许继承bean的配置，被继承的bean称为`父bean`，继承这个父bean的bean称为`子bean`
- 子bean从父bean中继承配置，包括bean的属性配置
- 子bean也可以覆盖从父bean中继承过来的配置
- 父bean可以作为配置模板，也可以作为Bean实例，若只想把父bean作为模板，可以设置`<bean>`的`abstract`属性为`true`，这样Spring将不会实例化这个Bean
- 并不是`<bean>`元素里的所有属性都会被继承，比如：`autowire`、`abstract`等
- 也可以忽略父Bean的class属性，让子bean指定自己的类，而共享相同的属性配置。但此时**abstract必须设置为true**

```xml
<!-- bean 配置的继承：使用 bean 的 parent 属性指定哪个 bean 的配置 -->
<bean id="" parent=""></bean>

<!-- 抽象bean： bean 的 abstract 属性为 true 的bean。这样的 bean 不能被 IOC 容器实例化，只能用来被继承设置
若某一个 bean 的 class 属性没有被指定，则该 bean 必须是一个抽象 bean -->
<bean id="" abstract=""></bean>
```

##### bean 的依赖

- Spring 允许用户通过`depends-on`属性设定Bean前置依赖的Bean，前置依赖的Bean会在本Bean实例化之前创建好。
- 如果前置依赖于多个Bean，则可以通过逗号，空格、或的方式配置Bean的名称。

```xml
<bean id="" class="" depends-on=“”></bean>
```

##### bean的作用域：singleton；prototype；WEB 环境作用域

```xml
<!--
  使用 bean 的 scope 属性来配置 bean 的作用域
  singleton: 默认值。容器初始时创建 bean 的实例，在整个容器的生命周期内只创建这一个 bean。单例的。
  prototype： 原型的。容器初始化时不创建 bean 的实例。而在每次请求时都创建一个新的 Bean 实例，并返回。
 -->
<bean id="" class="" scope=""></bean>
```

## 基于注解的方式

### 基于注解配置 Bean

#### 在 classpath 中扫描组件
- 组件扫描：Spring 能够从`classpath`下自动扫描，侦测和实例化具有特定注解的组件。
- 特定组件包括：
  + `@Component`：基本注解，标识了一个受 Spring 管理的组件
  + `@Respository`：标识持久层组件
  + `@Service`：标识服务层（业务层）组件
  + `@Controller`：标识表现层组件
  **以上均为建议标识内容**
- 对于扫描到的组件，Spring有`默认的命名策略`：使用非限定类名，第一个字母小写。`也可以在注解中通过value属性值标识组件的名称`。
- 当在组件类上使用了特定的注解之后，还需要在Spring的配置文件中声明：`<context:component-scan>`:
  + base-package 属性指定一个需要扫描的基类包，Spring容器将会扫描这个基类包里及其子包中的所有类。
  + 当需要扫描多个包时，可以使用逗号分隔。
  + 如果仅希望扫描特定的类而非基包下的所有类，可使用`resource-pattern`属性过滤特定的类，示例：
  + `use-default-filters`属性指定是否使用默认的过滤策略。

  ```xml
  <context:
    component-scan base-package="com.whtis.spring.beans"
    resource-pattern="autowire/*.class" use-default-filters="false">
  </context:component-scan>
  ```

  + `<context:include-filter>` 子节点表示要包含的目标类
  + `<context:exclude-filter>` 子节点表示要排除在外的目标类
  + `<context:component-scan>` 下可以拥有若干个`<context:include-filter>`和`<context:exclude-filter>`子节点
  + `<context:include-filter>`和`<context:exclude-filter>`子节点支持多种类型的过滤表达式：
    * `annotation`： 所有标注了`XxxAnnotation`的类。该类型采用目标类是否标注了某个注解进行过滤。
    * `assinable`：所有继承或扩展`XxxService`的类。该类型采用目标类是否继承或扩展某个特定类进行过滤。

#### 组件装配
如果一个组件中包含其他组件，仅靠配置`classpath`会导致无法找到组件中的其他组件从而报`nullPointException`错误。
- `<context:component-scan>`元素还会自动注册`AutowiredAnnotationBeanPostProcessor`实例，该实例可以自动装配具有`@Autowired`和`@Resource`、`@Inject`注解的属性。
   + `@Autowired`注解自动装配具有兼容类型的单个Bean属性
      * 构造器，普通字段（即使是非`public`），一切具有参数的方法都可以应用`@Autowired`注解。
      * 默认情况下，所有使用`@Autowired`注解的属性都需要被设置。当`Spring`找不到匹配的Bean装配属性时，会抛出异常。**若某一属性允许不被设置，可以设置@Autowired注解的`required`属性为`false`。**
      * 默认情况下，当 IOC 容器里存在多个类型兼容的Bean时，通过类型的自动装配将无法工作。此时可以在`@Qualifier`注解里提供Bean的名称。Spring允许对方法的入参标注`@Qualifiter`以指定注入Bean的名称。
      * `@Autowired` 注解也可以应用在`数组类型`的属性上，此时Spring将会把所有匹配的Bean进行自动装配
      * `@Autowired` 注解也可以应用在`集合属性`上，此时Spring读取该集合的类型信息，然后自动装配所有与之兼容的Bean
      * `@Autowired` 注解也可以应用在`java.util.Map`上，若该Map的键值为String，那么Spring将自动装配与Map值类型兼容的Bean，此时Bean的名称作为键值。
    + Spring 还支持`@Resource`和`@Inject`注解，这两个注解和`@Autowired`注解的功能类似
      * `@Resource` 注解要求提供一个 Bean 名称的属性，若该属性为空，则自动采用标注处的变量或方法名作为 Bean 的名称。
      * `@Inject`和`Autowired`注解一样也是按类型匹配注入的Bean，但没有`required`属性。
**建议使用`Autowired`注解**


# IOC 容器中 Bean 的生命周期

- Spring IOC 容器可以管理Bean的生命周期，Spring允许在Bean生命周期的特定点执行定制的任务。
- 管理过程：
  + 通过构造器或工厂方法创建Bean实例
  + 为Bean的属性设置值和对其他Bean的引用
  + 调用Bean的初始化方法
  + Bean可以使用了
  + 当容器关闭时，调用Bean的销毁方法
- 在Bean的声明里设置`init-method`和`destory-method`属性，为Bean指定初始化和销毁方法。
- 创建Bean的后置处理器
  + Bean后置处理器允许在调用初始化方法前后对Bean进行额外的处理
  + Bean后置处理器对IOC容器里的所有Bean实例逐一处理，而非单一实例。其典型应用是：检查Bean属性的正确性或根据特定的标准更改Bean的属性
  + 对Bean后置处理器而言，需要实现`org.springframework.beans.factory.config`的`BeanPostProcessor`接口。在初始化方法被调用前后，Spring将每个Bean实例分别传递给上述接口的以下两个方法：
    * `postProcessBeforeInitialization()`
    * `postProcessAfterInitialization()`

  ```xml
  <!--
    实现 BeanPostProcessor 接口，并具体实现
    public Object postProcessBeforeInitialization(Object o, String s) init-method 之前被调用
    public Object postProcessAfterInitialization(Object o, String s) init-method 之后被调用
    的实现

    bean: bean 实例本身
    beanName: IOC 容器配置的 bean 的名字
    返回值：是实际上返回给用户的那个 Bean，注意： 可以在以上两个方法中修改返回的 bean，甚至返回一个新的 bean
   -->
  <!-- 配置 Bean 的后置处理器： 不需要配置 id， IOC 容器自动识别是一个 BeanPostProcessor -->
  <bean class="myBeanPostProcessor"></bean>
  ```

  ---
  <div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
  <p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
  </div>
