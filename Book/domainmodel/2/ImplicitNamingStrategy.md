#隐式命名策略


    When an entity does not explicitly name the database table that it maps to, we need
    to implicitly determine that table name.  Or when a particular attribute does not explicitly name
    the database column that it maps to, we need to implicitly determine that column name.  There are
    examples of the role of the `org.hibernate.boot.model.naming.ImplicitNamingStrategy` contract to
    determine a logical name when the mapping did not provide an explicit name.

[Implicit Naming Strategy Diagram](/Book/images/domain/naming/implicit_naming_strategy_diagram.svg)
![](/Book/images/domain/naming/implicit_naming_strategy_diagram.svg)

    Hibernate defines multiple ImplicitNamingStrategy implementations out-of-the-box.  Applications
    are also free to plug-in custom implementations.

    </div>
    <div class="paragraph">

    There are multiple ways to specify the ImplicitNamingStrategy to use.  First, applications can specify
    the implementation using the `hibernate.implicit_naming_strategy` configuration setting which accepts:

    </div>
    <div class="ulist">

*   pre-defined "short names" for the out-of-the-box implementations
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`default`</dt>
    <dd>
    for `org.hibernate.boot.model.naming.ImplicitNamingStrategyJpaCompliantImpl` - an alias for `jpa`
    </dd>
    <dt class="hdlist1">`jpa`</dt>
    <dd>
    for `org.hibernate.boot.model.naming.ImplicitNamingStrategyJpaCompliantImpl` - the JPA 2.0 compliant naming strategy
    </dd>
    <dt class="hdlist1">`legacy-hbm`</dt>
    <dd>
    for `org.hibernate.boot.model.naming.ImplicitNamingStrategyLegacyHbmImpl` - compliant with the original Hibernate NamingStrategy
    </dd>
    <dt class="hdlist1">`legacy-jpa`</dt>
    <dd>
    for `org.hibernate.boot.model.naming.ImplicitNamingStrategyLegacyJpaImpl` - compliant with the legacy NamingStrategy developed for JPA 1.0, which was unfortunately unclear in many respects regarding implicit naming rules.
    </dd>
    <dt class="hdlist1">`component-path`</dt>
    <dd>
    for `org.hibernate.boot.model.naming.ImplicitNamingStrategyComponentPathImpl` - mostly follows `ImplicitNamingStrategyJpaCompliantImpl` rules, except that it uses the full composite paths, as opposed to just the ending property part
    </dd>
    </dl>
    </div>
*   reference to a Class that implements the `org.hibernate.boot.model.naming.ImplicitNamingStrategy` contract
*   FQN of a class that implements the `org.hibernate.boot.model.naming.ImplicitNamingStrategy` contract
    </div>
    <div class="paragraph">

    Secondly, applications and integrations can leverage `org.hibernate.boot.MetadataBuilder#applyImplicitNamingStrategy`
    to specify the ImplicitNamingStrategy to use.  See
    [Bootstrap](#bootstrap) for additional details on bootstrapping.

    </div>
    </div>
    <div class="sect3">