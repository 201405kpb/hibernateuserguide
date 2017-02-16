### 23.21. Miscellaneous properties

    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 60%;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Property
    </td>
    <td class="tableblock halign-left valign-top">

    Example
    </td>
    <td class="tableblock halign-left valign-top">

    Purpose
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.dialect_resolvers`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names any additional [`DialectResolver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/dialect/spi/DialectResolver.html) implementations to  register with the standard [`DialectFactory`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/dialect/spi/DialectFactory.html)
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.session_factory_name`
    </td>
    <td class="tableblock halign-left valign-top">

    A JNDI name
    </td>
    <td class="tableblock halign-left valign-top">

    Setting used to name the Hibernate `SessionFactory`.
    Naming the `SessionFactory` allows for it to be properly serialized across JVMs as long as the same name is used on each JVM.

    If `hibernate.session_factory_name_is_jndi` is set to `true`, this is also the name under which the `SessionFactory` is bound into JNDI on startup and from which it can be obtained from JNDI.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.session_factory_name_is_jndi`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Does the value defined by `hibernate.session_factory_name` represent a JNDI namespace into which the `org.hibernate.SessionFactory` should be bound and made accessible?

    Defaults to `true` for backwards compatibility. Set this to `false` if naming a SessionFactory is needed for serialization purposes, but no writable JNDI context exists in the runtime environment or if the user simply does not want JNDI to be used.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.entitymanager_factory_name`
    </td>
    <td class="tableblock halign-left valign-top">

    By default, the persistence unit name is used, otherwise a randomly generated UUID
    </td>
    <td class="tableblock halign-left valign-top">

    Internally, Hibernate keeps track of all `EntityManagerFactory` instances using the `EntityManagerFactoryRegistry`. The name is used as a key to identify a given `EntityManagerFactory` reference.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.cfgfile`
    </td>
    <td class="tableblock halign-left valign-top">

    `hibernate.cfg.xml` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    XML configuration file to use to configure Hibernate.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.discard_pc_on_close`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    If true, the persistence context will be discarded (think `clear()` when the method is called.
    Otherwise, the persistence context will stay alive till the transaction completion: all objects will remain managed, and any change will be synchronized with the database (default to false, ie wait for transaction completion).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.metamodel.population`
    </td>
    <td class="tableblock halign-left valign-top">

    `enabled` or `disabled`, or `ignoreUnsupported` (default value)
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting that indicates whether to build the JPA types.

    </div>
    <div class="paragraph">

    Accepts three values:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">enabled</dt>
    <dd>

    Do the build

    </dd>
    <dt class="hdlist1">disabled</dt>
    <dd>

    Do not do the build

    </dd>
    <dt class="hdlist1">ignoreUnsupported</dt>
    <dd>

    Do the build, but ignore any non-JPA features that would otherwise result in a failure (e.g. `@Any` annotation).

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jpa.static_metamodel.population`
    </td>
    <td class="tableblock halign-left valign-top">

    `enabled` or `disabled`, or `skipUnsupported` (default value)
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting that controls whether we seek out JPA _static metamodel_ classes and populate them.

    </div>
    <div class="paragraph">

    Accepts three values:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">enabled</dt>
    <dd>

    Do the population

    </dd>
    <dt class="hdlist1">disabled</dt>
    <dd>

    Do not do the population

    </dd>
    <dt class="hdlist1">skipUnsupported</dt>
    <dd>

    Do the population, but ignore any non-JPA features that would otherwise result in the population failing (e.g. `@Any` annotation).

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.delay_cdi_access`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Defines delayed access to CDI `BeanManager`. Starting in 5.1 the preferred means for CDI bootstrapping is through [`ExtendedBeanManager`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/jpa/event/spi/jpa/ExtendedBeanManager.html).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.allow_update_outside_transaction`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting that allows to perform update operations outside of a transaction boundary.

    </div>
    <div class="paragraph">

    Accepts two values:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">true</dt>
    <dd>

    allows to flush an update out of a transaction

    </dd>
    <dt class="hdlist1">false</dt>
    <dd>

    does not allow

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.collection_join_subquery`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Setting which indicates whether or not the new JOINS over collection tables should be rewritten to subqueries.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.allow_refresh_detached_entity`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value when using Hibernate native bootstrapping) or `false` (default value when using JPA bootstrapping)
    </td>
    <td class="tableblock halign-left valign-top">

    Setting that allows to call `javax.persistence.EntityManager#refresh(entity)` or `Session#refresh(entity)` on a detached instance even when the `org.hibernate.Session` is obtained from a JPA `javax.persistence.EntityManager`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.event.merge.entity_copy_observer`
    </td>
    <td class="tableblock halign-left valign-top">

    `disallow` (default value), `allow`, `log` (testing purpose only) or fully-qualified class name
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting that specifies how Hibernate will respond when multiple representations of the same persistent entity ("entity copy") is detected while merging.

    </div>
    <div class="paragraph">

    The possible values are:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">disallow (the default)</dt>
    <dd>

    throws `IllegalStateException` if an entity copy is detected

    </dd>
    <dt class="hdlist1">allow</dt>
    <dd>

    performs the merge operation on each entity copy that is detected

    </dd>
    <dt class="hdlist1">log</dt>
    <dd>

    (provided for testing only) performs the merge operation on each entity copy that is detected and logs information about the entity copies.
    This setting requires DEBUG logging be enabled for [`EntityCopyAllowedLoggedObserver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/event/internal/EntityCopyAllowedLoggedObserver.html).

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    In addition, the application may customize the behavior by providing an implementation of [`EntityCopyObserver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/event/spi/EntityCopyObserver.html) and setting `hibernate.event.merge.entity_copy_observer` to the class name.
    When this property is set to `allow` or `log`, Hibernate will merge each entity copy detected while cascading the merge operation.
    In the process of merging each entity copy, Hibernate will cascade the merge operation from each entity copy to its associations with `cascade=CascadeType.MERGE` or `CascadeType.ALL`.
    The entity state resulting from merging an entity copy will be overwritten when another entity copy is merged.

    </div>
    <div class="paragraph">

    For more details, check out the [Merge gotchas](#pc-merge-gotchas) section.

    </div></div></td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">