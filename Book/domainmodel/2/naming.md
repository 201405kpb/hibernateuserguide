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
>历史上的命名策略

>Historically Hibernate defined just a single `org.hibernate.cfg.NamingStrategy`. That singular
NamingStrategy contract actually combined the separate concerns that are now modeled individually as ImplicitNamingStrategy and PhysicalNamingStrategy.
> 历史上Hibernate只实现了一个org.hibernate.cfg.NamingStrategy接口。 实际上，单一的NamingStrategy接口将ImplicitNamingStrategy接口和PhysicalNamingStrategy接口的结合在一起。

>Also, the NamingStrategy contract was often not flexible enough to properly apply a given naming"rule", either because the API lacked the information to decide or because the API was honestlynot well defined as it grew.
>此外，NamingStrategy接口通常不够灵活，无法正确应用给定的命名“规则”，这是因为Hibernate API 缺少必要的信息，或者因为 Hibernate API随着它的增长而未被明确定义。

>Due to these limitation, `org.hibernate.cfg.NamingStrategy` has been deprecated and then removedin favor of ImplicitNamingStrategy and PhysicalNamingStrategy.
>由于这些限制，`org.hibernate.cfg.NamingStrategy`已被弃用，然后被移除,用ImplicitNamingStrategy接口和PhysicalNamingStrategy接口替换。

At the core, the idea behind each naming strategy is to minimize the amount of
repetitive information a developer must provide for mapping a domain model.
在核心，每个命名策略背后的想法是最小化开发人员必须提供的用于映射域模型的重复信息的量。

>![JPA Compatibility](/Book/images/org/hibernate/docbook/note.png)
>JPA defines inherent rules about implicit logical name determination. If JPA provider
portability is a major concern, or if you really just like the JPA-defined implicit
naming rules, be sure to stick with ImplicitNamingStrategyJpaCompliantImpl (the default)

>JPA定义了关于隐式逻辑名确定的固有规则。 如果JPA提供程序的可移植性是一个主要问题，或者如果你真的只是喜欢JPA定义的隐式命名规则，请务必坚持ImplicitNamingStrategyJpaCompliantImpl(默认的实现方式)

>Also, JPA defines no separation between logical and physical name. Following the JPA
specification, the logical name **is** the physical name. If JPA provider portability
is important, applications should prefer not to specify a PhysicalNamingStrategy.

>此外，JPA定义逻辑名和物理名之间没有分隔。 根据JPA规范，逻辑名称是物理名称。 如果JPA提供程序的可移植性很重要，应用程序应该不要指定PhysicalNamingStrategy。
