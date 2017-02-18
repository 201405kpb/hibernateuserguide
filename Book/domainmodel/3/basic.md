#基础类型

Basic value types usually map a single database column, to a single, non-aggregated Java type. Hibernate provides a number of built-in basic types, which follow the natural mappings recommended by the JDBC specifications.
基本值类型通常将单个数据库列映射到单个非聚合Java类型。Hibernate提供了许多内置的基本类型，它遵循JDBC规范推荐的自然映射。

Internally Hibernate uses a registry of basic types when it needs to resolve a specific `org.hibernate.type.Type`.
当Hibernate需要解析一个特定的`org.hibernate.type.Types`类型的时候，Hibernate在内部使用基本类型的注册表。