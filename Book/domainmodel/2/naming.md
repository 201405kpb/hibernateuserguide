#命名策略

Part of the mapping of an object model to the relational database is mapping names from the object model to the corresponding database names.Hibernate looks at this as 2 stage process:
对象模型到关系数据库的映射的一部分是将名称从对象模型映射到相应的数据库名称。Hibernate将这看作是2阶段过程：

* The first stage is determining a proper logical name from the domain model mapping. A
logical name can be either explicitly specified by the user (using `@Column` or
`@Table` e.g.) or it can be implicitly determined by Hibernate through an
[ImplicitNamingStrategy](/Book/domainmodel/2/ImplicitNamingStrategy.md) contract.
* 第一阶段是从域模型映射确定合适的逻辑名。 一个逻辑名可以由用户显式指定（例如,使用`@ Column`或`@ Table`注解）或者它可以由Hibernate通过隐式确定[隐式命名策略](/Book/domainmodel/2/ImplicitNamingStrategy.md)。
* Second is the resolving of this logical name to a physical name which is defined
by the [PhysicalNamingStrategy](/Book/domainmodel/2/ImplicitNamingStrategy.md) contract.
* 第二是将该逻辑名称解析为通过[显示命名策略](/Book/domainmodel/2/PhysicalNamingStrategy.md)实现的物理名称。

>![Historical NamingStrategy contract](/Book/images/org/hibernate/docbook/note.png)
>Historical NamingStrategy contract
>历史命名策略

>Historically Hibernate defined just a single `org.hibernate.cfg.NamingStrategy`. That singular
NamingStrategy contract actually combined the separate concerns that are now modeled individually as ImplicitNamingStrategy and PhysicalNamingStrategy.


>Also, the NamingStrategy contract was often not flexible enough to properly apply a given naming
"rule", either because the API lacked the information to decide or because the API was honestly
not well defined as it grew.

>Due to these limitation, `org.hibernate.cfg.NamingStrategy` has been deprecated and then removed
in favor of ImplicitNamingStrategy and PhysicalNamingStrategy.

At the core, the idea behind each naming strategy is to minimize the amount of
repetitive information a developer must provide for mapping a domain model.


JPA defines inherent rules about implicit logical name determination. If JPA provider
portability is a major concern, or if you really just like the JPA-defined implicit
naming rules, be sure to stick with ImplicitNamingStrategyJpaCompliantImpl (the default)

Also, JPA defines no separation between logical and physical name. Following the JPA
specification, the logical name **is** the physical name. If JPA provider portability
is important, applications should prefer not to specify a PhysicalNamingStrategy.
