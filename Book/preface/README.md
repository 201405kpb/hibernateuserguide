# 前言

Working with both Object-Oriented software and Relational Databases can be cumbersome and time consuming. Development costs are significantly higher due to a paradigm mismatch between how data is represented in objects versus relational databases. Hibernate is an Object/Relational Mapping solution for Java environments. The term Object/Relational Mapping refers to the technique of mapping data from an object model representation to a relational data model representation (and visa versa).

在今日的企业环境中，把面向对象的软件和关系数据库一起使用可能是相当麻烦、浪费时间的。由于数据在对象和关系数据库之间的表示方式不匹配，开发成本显着增高。Hibernate是一个面向Java环境的对象/关系数据库映射工具。对象/关系数据库映射(object/relational mapping (ORM))这个术语表示一种技术，用来将数据从对象模型表示映射到关系数据模型表示（反之亦然）的技术。

Hibernate not only takes care of the mapping from Java classes to database tables \(and from Java data types to SQL data types\), but also provides data query and retrieval facilities. It can significantly reduce development time otherwise spent with manual data handling in SQL and JDBC. Hibernate’s design goal is to relieve the developer from 95% of common data persistence-related programming tasks by eliminating the need for manual, hand-crafted data processing using SQL and JDBC. However, unlike many other persistence solutions, Hibernate does not hide the power of SQL from you and guarantees that your investment in relational technology and knowledge is as valid as always.

Hibernate不仅负责从Java类到数据库表（以及从Java数据类型到SQL数据类型）的映射，而且还提供数据查询和检索功能。它可以显着减少开发时间，否则在SQL和JDBC中进行手动数据处理。 Hibernate的设计目标是通过消除对使用SQL和JDBC的手动，手工数据处理的需求，从95％的常见数据持久性相关的编程任务中减轻开发人员。但是，与许多其他持久性解决方案不同，Hibernate不会隐藏SQL的强大功能，并保证您对关系技术和知识的投资与始终一样有效。

Hibernate may not be the best solution for data-centric applications that only use stored-procedures to implement the business logic in the database, it is most useful with object-oriented domain models and business logic in the Java-based middle-tier. However, Hibernate can certainly help you to remove or encapsulate vendor-specific SQL code and will help with the common task of result set translation from a tabular representation to a graph of objects.

对于仅使用存储过程来实现数据库中的业务逻辑的以数据为中心的应用程序，Hibernate可能不是最好的解决方案，它对于基于Java的中间层中的面向对象的域模型和业务逻辑最为有用。但是，Hibernate当然可以帮助您删除或封装供应商特定的SQL代码，并且将帮助从表格表示到对象图的结果集翻译的常见任务。

