title: spring4学习笔记（四）--Spring-4.x新特性：泛型依赖注入
date: 2017-10-13 15:09:57
categories: java web
tags: [spring4,java]
---

Spring4 添加了泛型依赖注入的功能。具体的，从下面这张图来看：
![spring4.x泛型依赖注入](spring4.x泛型依赖注入.jpg)

假使我们创建了两个泛型基类`BaseService<T>`和`BaseRepository<T>`，并分别进行了实现`UserService<User>`和`UserRepository<User>`，如果在泛型基类中存在如下代码：

```java
public class BaseService<T> {

    @Autowired
    protected BaseRepository baseRepository;

    void add() {
        System.out.println(baseRepository);
    }
}
```

而`UserService<User>`类仅仅实现了`BaseService`的接口，没有重写`add`方法：

```java
@service
public class UserService<User> implement BaseService{

}
```

`UserRepository<User>`与`UserService<User>`类似。将这些Bean交给spring IOC进行管理，创建`main`方法：

```java
public class Main{
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("xxx.xml");
        UserService userService = (UserService) ctx.getBean("userService");
        //调用userService的add方法
        UserService.add();
    }

}
```

可以看到`System.out.println(baseRepository);`的输出结果是一个`userRepository`类。
由此可知，spring4自动将`User`类传入了`baseRepository`的继承类中。

---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
