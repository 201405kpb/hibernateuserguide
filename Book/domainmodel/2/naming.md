#命名策略

Part of the mapping of an object model to the relational database is
mapping names from the object model to the corresponding database names.
Hibernate looks at this as 2 stage process:


* The first stage is determining a proper logical name from the domain model mapping. A
logical name can be either explicitly specified by the user (using `@Column` or
`@Table` e.g.) or it can be implicitly determined by Hibernate through an
[ImplicitNamingStrategy](#ImplicitNamingStrategy) contract.
* Second is the resolving of this logical name to a physical name which is defined
by the [PhysicalNamingStrategy](#PhysicalNamingStrategy) contract.
Historical NamingStrategy contract


Historically Hibernate defined just a single `org.hibernate.cfg.NamingStrategy`. That singular
NamingStrategy contract actually combined the separate concerns that are now modeled individually
as ImplicitNamingStrategy and PhysicalNamingStrategy.


Also, the NamingStrategy contract was often not flexible enough to properly apply a given naming
"rule", either because the API lacked the information to decide or because the API was honestly
not well defined as it grew.

Due to these limitation, `org.hibernate.cfg.NamingStrategy` has been deprecated and then removed
in favor of ImplicitNamingStrategy and PhysicalNamingStrategy.

At the core, the idea behind each naming strategy is to minimize the amount of
repetitive information a developer must provide for mapping a domain model.


JPA defines inherent rules about implicit logical name determination. If JPA provider
portability is a major concern, or if you really just like the JPA-defined implicit
naming rules, be sure to stick with ImplicitNamingStrategyJpaCompliantImpl (the default)

Also, JPA defines no separation between logical and physical name. Following the JPA
specification, the logical name **is** the physical name. If JPA provider portability
is important, applications should prefer not to specify a PhysicalNamingStrategy.
