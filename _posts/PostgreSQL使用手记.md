title: PostgreSQL使用手记
date: 2016-09-14 22:29:13
categories: SQL
tags: [java,数据库，PostgreSQL]

---

### 写在前面
<blockcode>
&emsp;&emsp;因为项目需要，要将本来的`mybatis+mysql`改为`mybatis+postgresql`,使用途中难免遇到坑，以下是我遇到的，记录下来供参考。好久没写blog，感觉都快长草了，目前在想要不要开一个分类，写点日常废话😂
</blockcode>
&emsp;&emsp;好了，言归正传。

### 为什么要用PostgreSQL
为什么要用呢,上面已经说了，项目需要。postgresql经常被拿来和mysql进行比较，postgresql可以用来存储较大的对象数据（文本、视频、图片等），符合项目要求。

### 什么是PostgreSQL
念都不知道怎么念的一个词，确实第一感觉没有mysql好。看下官方的说法：
>PostgreSQL支持大部分 SQL标准并且提供了许多其他现代特性：复杂查询、外键、触发器、视图、事务完整性、MVCC。同样，PostgreSQL 可以用许多方法扩展，比如， 通过增加新的数据类型、函数、操作符、聚集函数、索引。免费使用、修改、和分发 PostgreSQL，不管是私用、商用、还是学术研究使用。(摘自百度百科)

### 使用版本
本文总结基于PostgreSQL 9.4版本

### 安装及使用
推荐下阮一峰写的这篇博客[PostgreSQL新手入门](http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html)

### 使用中出现的错误及解决方案
#### 错误一： `java.sql.SQLException: No suitable driver found for jdbc:postgresql://yourhost:port`
- 可能的错误原因：
  + 使用的postgresql的驱动与当前使用的jdk版本不兼容。具体兼容情况参见官方文档，本文仅作摘录：
  + 系统环境没有配置classpath路径，使的程序无法找到驱动的位置。（使用java IDE编写代码时一般不会出现这种情况）
  + **连接jdbc时url没写正确，一定要再三检查下正确的url**（这一条很恶心，url错误时的提示我遇到过两种，一种是本条目列出的错误，另一种是提示字符编码有问题）

#### 错误二： `Error querying database.  Cause: org.postgresql.util.PSQLException: ERROR: CREATE DATABASE cannot run inside a transaction block`
- 可能的错误原因：
  + 根据网上查到的资料，单独使用postgresql时，出现这个错误的原因是因为没有打开自动提交。
     ```java
     		c = DriverManager.getConnection("jdbc:postgresql://yourhost", "username", "password");
            c.setAutoCommit(true); // 把自动提交设置为true
     ```
  + 我在项目中单独使用Mybatis进行事务管理，管理`PostgreSQL`时也会出现这个错误，因为Mybatis封装了`java.sql.*`相关的类,因此无法按照上面方式进行更改，在Mybatis中打开一个会话时，也应该将其设置为自动提交（mysql无需设置）：
     ```java
     		SqlSession sqlSession = sqlSessionFactory.openSession(true);
     ```
  + **总结：出现这个问题，网上给的解决方法大多是因为没有打开自动提交，但是仅仅在portgresql控制台上执行`SET AUTOCOMMIT = on`是无用的，至于为什么需要打开自动提交才能创建数据库，这个不太清楚，可能跟`PostgreSQL`自己的事务管理逻辑有关，而且这样改动后，暂时不知道会不会对之后的数据库操作造成影响**

#### 错误三：在拷贝excel中数据到pgsql中时，使用语句时有以下问题需要注意：

```java
COPY excel.csv FROM 'address' WITH (FORMAT CSV, HEADER TRUE, QUOTE '"', DELIMITER ',', ENCODING 'UTF8' );
```

- `ERROR: must be superuser to COPY to or from a file,建议：Anyone can COPY to stdout or from stdin. psql's copy command also works for anyone.`
  + 这是因为当前登陆pgsql的用户不具有向数据库copy数据的权限所致。需要切换到superuser进行此操作。
- `ERROR: invalid input syntax for integer: `
  + 这个错误是网上使用`COPY excel.csv FROM 'address' WITH DELIMITER ','`所致，如果在copy命令中指定选项header为true. 另外，将format指定为csv, 文件格式指定为utf8，就能避免这错误。

### 使用PostgreSQL存储大文件
待编辑




---
<div align="center" style="color:red;width=80px;height:90px;" onmouseout="this.style.border='1px solid blue'" onmouseover="this.style.border='none'">
<p style="font-weight:bold;font-style:italic;">本文章首发[www.whtis.com](http://www.whtis.com)，转载请注明出处</p>
</div>
