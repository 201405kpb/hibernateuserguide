 ### 23.12. Transactions properties

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

    `hibernate.transaction.jta.platform`
    </td>
    <td class="tableblock halign-left valign-top">

    `JBossAS`, `BitronixJtaPlatform`
    </td>
    <td class="tableblock halign-left valign-top">

    Names the [`JtaPlatform`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/transaction/jta/platform/spi/JtaPlatform.html) implementation to use for integrating with JTA systems.
    Can reference either a [`JtaPlatform`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/transaction/jta/platform/spi/JtaPlatform.html) instance or the name of the [`JtaPlatform`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/transaction/jta/platform/spi/JtaPlatform.html) implementation class
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jta.prefer_user_transaction`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Should we prefer using the `org.hibernate.engine.transaction.jta.platform.spi.JtaPlatform#retrieveUserTransaction` over using `org.hibernate.engine.transaction.jta.platform.spi.JtaPlatform#retrieveTransactionManager`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.transaction.jta.platform_resolver`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the [`JtaPlatformResolver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/transaction/jta/platform/spi/JtaPlatformResolver.html) implementation to use.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jta.cacheTransactionManager`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    A configuration value key used to indicate that it is safe to cache.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jta.cacheUserTransaction`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    A configuration value key used to indicate that it is safe to cache.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.transaction.flush_before_completion`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Causes the session be flushed during the before completion phase of the transaction. If possible, use built-in and automatic session context management instead.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.transaction.auto_close_session`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Causes the session to be closed during the after completion phase of the transaction. If possible, use built-in and automatic session context management instead.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.transaction.coordinator_class`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Names the implementation of [`TransactionCoordinatorBuilder`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/resource/transaction/spi/TransactionCoordinatorBuilder.html) to use for creating [`TransactionCoordinator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/resource/transaction/spi/TransactionCoordinator.html) instances.

    </div>
    <div class="paragraph">

    Can be

    </div>
    <div class="ulist">

*   `TransactionCoordinatorBuilder` instance
*   `TransactionCoordinatorBuilder` implementation `Class` reference
*   `TransactionCoordinatorBuilder` implementation class name (fully-qualified name) or short name
    </div>
    <div class="paragraph">

    The following short names are defined for this setting:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`jdbc`</dt>
    <dd>

    Manages transactions via calls to `java.sql.Connection` (default for non-JPA applications)

    </dd>
    <dt class="hdlist1">`jta`</dt>
    <dd>

    Manages transactions via JTA. See [Java EE bootstrapping](#bootstrap-jpa-compliant)

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    If a JPA application does not provide a setting for `hibernate.transaction.coordinator_class`, Hibernate will
    automatically build the proper transaction coordinator based on the transaction type for the persistence unit.

    </div>
    <div class="paragraph">

    If a non-JPA application does not provide a setting for `hibernate.transaction.coordinator_class`, Hibernate
    will use `jdbc` as the default. This default will cause problems if the application actually uses JTA-based transactions.
    A non-JPA application that uses JTA-based transactions should explicitly set `hibernate.transaction.coordinator_class=jta`
    or provide a custom [`TransactionCoordinatorBuilder`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/resource/transaction/TransactionCoordinatorBuilder.html) that builds a [`TransactionCoordinator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/resource/transaction/TransactionCoordinator.html) that properly coordinates with JTA-based transactions.

    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jta.track_by_thread`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    A transaction can be rolled back by another thread ("tracking by thread") and not the original application.
    Examples of this include a JTA transaction timeout handled by a background reaper thread.

    The ability to handle this situation requires checking the Thread ID every time Session is called, so enabling this can certainly have a performance impact.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.transaction.factory_class`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    This is a legacy setting that&#8217;s been deprecated and you should use the `hibernate.transaction.jta.platform` instead.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">