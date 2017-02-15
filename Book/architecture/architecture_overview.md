# 体系结构概述

![Data Access Layers](/Book/images/architecture/data_access_layers.svg)

Hibernate, as an ORM solution, effectively "sits between" the Java application data access layer and the Relational Database, as can be seen in the diagram above.
Hibernate作为一个ORM解决方案，有效地“位于”Java应用程序数据访问层和关系数据库之间，如上图所示。
The Java application makes use of the Hibernate APIs to load, store, query, etc its domain data.
Java应用程序利用Hibernate API来加载，存储，查询等其域数据。
Here we will introduce the essential Hibernate APIs.
这里我们将介绍基本的Hibernate API。
This will be a brief introduction; we will discuss these contracts in detail later.
这将是一个简单的介绍; 我们将在后面详细讨论这些合同。

As a JPA provider, Hibernate implements the Java Persistence API specifications and the association between JPA interfaces and Hibernate specific implementations can be visualized in the following diagram:
作为JPA提供者，Hibernate实现了Java Persistence API规范，JPA接口和Hibernate特定实现之间的关联可以在下图中可视化：


![image](/Book/images/architecture/JPA_Hibernate.svg)
>SessionFactory (`org.hibernate.SessionFactory`)

A thread-safe (and immutable) representation of the mapping of the application domain model to a database.
Acts as a factory for `org.hibernate.Session` instances. The `EntityManagerFactory` is the JPA equivalent of a `SessionFactory` and basically those two converge into the same `SessionFactory` implementation.
A `SessionFactory` is very expensive to create, so, for any given database, the application should have only one associated `SessionFactory`.
The `SessionFactory` maintains services that Hibernate uses across all `Session(s)` such as second level caches, connection pools, transaction system integrations, etc.
应用程序域模型到数据库的映射的线程安全（和不可变）表示。
作为`org.hibernate.Session`实例的工厂。 `EntityManagerFactory`是JPA等价于一个`SessionFactory`，基本上这两个收敛到同一个`SessionFactory`实现中。
一个`SessionFactory`是非常昂贵的创建，所以，对于任何给定的数据库，应用程序应该只有一个相关联的`SessionFactory`。
SessionFactory保持Hibernate在所有“会话”中使用的服务，例如第二级缓存，连接池，事务系统集成等。

>Session (`org.hibernate.Session`)
A single-threaded, short-lived object conceptually modeling a "Unit of Work".
In JPA nomenclature, the `Session` is represented by an `EntityManager`.
Behind the scenes, the Hibernate `Session` wraps a JDBC `java.sql.Connection` and acts as a factory for `org.hibernate.Transaction` instances.
It maintains a generally "repeatable read" persistence context (first level cache) of the application domain model.
一个单线程，短命的对象概念地建模一个“工作单位”。
在JPA命名法中，`Session`由`EntityManager`表示。
在幕后，Hibernate`Session`包装一个JDBC`java.sql.Connection`，并作为`org.hibernate.Transaction`实例的工厂。
它维护应用程序域模型的一般“可重复读”持久性上下文（第一级缓存）。

>Transaction (`org.hibernate.Transaction`)
A single-threaded, short-lived object used by the application to demarcate individual physical transaction boundaries.
`EntityTransaction` is the JPA equivalent and both act as an abstraction API to isolate the application from the underlying transaction system in use (JDBC or JTA).
应用程序使用的单线程，短期对象来划分单个物理事务边界。
`EntityTransaction`是JPA等价物，并且都充当抽象API，以将应用程序与正在使用的底层事务系统（JDBC或JTA）隔离。
