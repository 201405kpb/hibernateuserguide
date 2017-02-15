#隐式命名策略

When an entity does not explicitly name the database table that it maps to, we need
to implicitly determine that table name. Or when a particular attribute does not explicitly name
the database column that it maps to, we need to implicitly determine that column name. There are
examples of the role of the `org.hibernate.boot.model.naming.ImplicitNamingStrategy` contract to
determine a logical name when the mapping did not provide an explicit name.

![](/Book/images/domain/naming/implicit_naming_strategy_diagram.svg)

Hibernate defines multiple ImplicitNamingStrategy implementations out-of-the-box. Applications
are also free to plug-in custom implementations.

There are multiple ways to specify the ImplicitNamingStrategy to use. First, applications can specify
the implementation using the `hibernate.implicit_naming_strategy` configuration setting which accepts:

