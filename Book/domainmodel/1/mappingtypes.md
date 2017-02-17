# 类型映射

Hibernate understands both the Java and JDBC representations of application data.  
The ability to read/write this data from/to the database is the function of a Hibernate type.  
A type, in this usage, is an implementation of the `org.hibernate.type.Type` interface.  
This Hibernate type also describes various aspects of behavior of the Java type such as how to check for equality, how to clone values, etc.
Hibernate可以解析用Java和JDBC表示应用程序数据。Hibernate拥有从数据库读取/写入数据的方法。在这个用法中，一种类型是通过`org.hibernate.type.Type`接口的实现。这种Hibernate类型还描述了Java类型的行为的各个方面，例如如何检查相等性，如何克隆值等。

The Hibernate type is neither a Java type nor a SQL data type. It provides information about both of these as well as understanding marshalling between.
Hibernate类型既不是Java类型也不是SQL数据类型。它提供关于这两者的信息以及两者之间的关系。

When you encounter the term type in discussions of Hibernate, it may refer to the Java type, the JDBC type, or the Hibernate type, depending on context.
当在Hibernate的使用中遇到术语类型时，根据上下文，它可能引用Java类型，JDBC类型或Hibernate类型。

To help understand the type categorizations, let’s look at a simple table and domain model that we wish to map.
为了理解类型的分类，让我们看一个简单的按照我们希望的方式处理的表和域模型的映射例子。

```sql
create table Contact (
    id integer not null,
    first varchar(255),
    last varchar(255),
    middle varchar(255),
    notes varchar(255),
    starred boolean not null,
    website varchar(255),
    primary key (id)
)
```
```java
public static class Contact {

    @Id
    private Integer id;

    private Name name;

    private String notes;

    private URL website;

    private boolean starred;

    //省略Getters和setters方法
}

@Embeddable
public class Name {

    private String first;

    private String middle;

    private String last;

    // 省略 getters和setters方法
}
```

In the broadest sense, Hibernate categorizes types into two groups:
在最广泛的意义上，Hibernate将类型分为两组：

* [Value types](categorizationvalue.md)
* [值类型](categorizationvalue.md)
* [Entity types](categorizationentity.md)
* [实体类型](categorizationentity.md)



