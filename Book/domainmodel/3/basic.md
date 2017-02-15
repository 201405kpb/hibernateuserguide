#基础类型

Basic value types usually map a single database column, to a single, non-aggregated Java type.
Hibernate provides a number of built-in basic types, which follow the natural mappings recommended by the JDBC specifications.

Internally Hibernate uses a registry of basic types when it needs to resolve a specific `org.hibernate.type.Type`.