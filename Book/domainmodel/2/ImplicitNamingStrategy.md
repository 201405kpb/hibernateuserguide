#隐式命名策略

When an entity does not explicitly name the database table that it maps to, we need
to implicitly determine that table name. Or when a particular attribute does not explicitly name
the database column that it maps to, we need to implicitly determine that column name. There are
examples of the role of the `org.hibernate.boot.model.naming.ImplicitNamingStrategy` contract to
determine a logical name when the mapping did not provide an explicit name.
当实体没有显式地命名它映射到的数据库表时，我们需要以隐式确定表名。 或者当一个特定的属性没有显式地命名它映射到的数据库列，我们需要隐式确定列名。`org.hibernate.boot.model.naming.ImplicitNamingStrategy`接口的作用是用来确定映射未提供显式名称时的逻辑名。

![implicit_naming_strategy_diagram](/Book/images/domain/naming/implicit_naming_strategy_diagram.svg)

Hibernate defines multiple ImplicitNamingStrategy implementations out-of-the-box. Applications are also free to plug-in custom implementations.
Hibernate定义了多个ImplicitNamingStrategy实现。 应用程序还可以免费插入自定义实现。

There are multiple ways to specify the ImplicitNamingStrategy to use. First, applications can specify
the implementation using the `hibernate.implicit_naming_strategy` configuration setting which accepts:
有多种方法可以指定要使用的ImplicitNamingStrategy。 首先，应用程序可以使用hibernate.implicit_naming_strategy配置设置指定实现，该设置接受：

* pre-defined "short names" for the out-of-the-box implementations
* 预定义的“短名称”用于开箱即用的实现

>`default`
>for `org.hibernate.boot.model.naming.ImplicitNamingStrategyJpaCompliantImpl` - an alias for `jpa`
>默认为`org.hibernate.boot.model.naming.ImplicitNamingStrategyJpaCompliantImpl`，一个`jpa`的别名
>`jpa`
>for `org.hibernate.boot.model.naming.ImplicitNamingStrategyJpaCompliantImpl` - the JPA 2.0 compliant naming strategy
`jpa`为 `org.hibernate.boot.model.naming.ImplicitNamingStrategyJpaCompliantImpl`，JPA2.0中复杂的命名策略
>`legacy-hbm`
>for `org.hibernate.boot.model.naming.ImplicitNamingStrategyLegacyHbmImpl` - compliant with the original Hibernate NamingStrategy
>`legacy-hbm`为 `org.hibernate.boot.model.naming.ImplicitNamingStrategyLegacyHbmImpl`,符合原来的Hibernate命名策略
>`legacy-jpa`
>for `org.hibernate.boot.model.naming.ImplicitNamingStrategyLegacyJpaImpl` - compliant with the legacy NamingStrategy developed for JPA 1.0, which was unfortunately unclear in many respects regarding implicit naming rules.
>`legacy-jpa`为 `org.hibernate.boot.model.naming.ImplicitNamingStrategyLegacyJpaImpl`，符合为JPA 1.0开发的传统NamingStrategy，这在关于隐式命名规则的许多方面是不清楚的。
>`component-path`
>for `org.hibernate.boot.model.naming.ImplicitNamingStrategyComponentPathImpl` - mostly follows `ImplicitNamingStrategyJpaCompliantImpl` rules, except that it uses the full composite paths, as opposed to just the ending property part
>`component-path`为`org.hibernate.boot.model.naming.ImplicitNamingStrategyComponentPathImpl`，主要遵循`ImplicitNamingStrategyJpaCompliantImpl'规则，除了它使用完整的复合路径，而不是仅仅结束属性部分

* reference to a Class that implements the `org.hibernate.boot.model.naming.ImplicitNamingStrategy` contract
* 引用实现org.hibernate.boot.model.naming.ImplicitNamingStrategy接口的类
* FQN of a class that implements the `org.hibernate.boot.model.naming.ImplicitNamingStrategy` contract
* 实现org.hibernate.boot.model.naming.ImplicitNamingStrategy`合约的类.

Secondly, applications and integrations can leverage `org.hibernate.boot.MetadataBuilder#applyImplicitNamingStrategy`
to specify the ImplicitNamingStrategy to use. See
[Bootstrap](/Book/bootstrap/README.md) for additional details on bootstrapping.
其次，应用程序可以利用`org.hibernate.boot.MetadataBuilder`接口的`applyImplicitNamingStrategy`方法指定要使用的ImplicitNamingStrategy。 有关引导的更多详细信息可以[参考这里](/Book/bootstrap/README.md)。


