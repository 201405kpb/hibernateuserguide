 ### 23.7. Query settings

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

    `hibernate.query.plan_cache_max_size`
    </td>
    <td class="tableblock halign-left valign-top">

    `2048` (default value)
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    The maximum number of entries including:

    </div>
    <div class="ulist">

*   [`HQLQueryPlan`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/query/spi/HQLQueryPlan.html)
*   [`FilterQueryPlan`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/query/spi/FilterQueryPlan.html)
*   [`NativeSQLQueryPlan`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/query/spi/NativeSQLQueryPlan.html)
    </div>
    <div class="paragraph">

    maintained by [`QueryPlanCache`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/query/spi/QueryPlanCache.html).

    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.query.plan_parameter_metadata_max_size`
    </td>
    <td class="tableblock halign-left valign-top">

    `128` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    The maximum number of strong references associated with `ParameterMetadata` maintained by [`QueryPlanCache`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/query/spi/QueryPlanCache.html).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.order_by.default_null_ordering`
    </td>
    <td class="tableblock halign-left valign-top">

    `none`, `first` or `last`
    </td>
    <td class="tableblock halign-left valign-top">

    Defines precedence of null values in `ORDER BY` clause. Defaults to `none` which varies between RDBMS implementation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.discriminator.force_in_select`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    For entities which do not explicitly say, should we force discriminators into SQL selects?
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.query.substitutions`
    </td>
    <td class="tableblock halign-left valign-top">

    `true=1,false=0`
    </td>
    <td class="tableblock halign-left valign-top">

    A comma-separated list of token substitutions to use when translating a Hibernate query to SQL.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.query.factory_class`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.hql.internal.ast.
    ASTQueryTranslatorFactory` (default value) or `org.hibernate.hql.internal.classic.
    ClassicQueryTranslatorFactory`
    </td>
    <td class="tableblock halign-left valign-top">

    Chooses the HQL parser implementation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.query.jpaql_strict_compliance`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Map from tokens in Hibernate queries to SQL tokens, such as function or literal names.

    Should we strictly adhere to JPA Query Language (JPQL) syntax, or more broadly support all of Hibernate&#8217;s superset (HQL)?

    Setting this to `true` may cause valid HQL to throw an exception because it violates the JPQL subset.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.query.startup_check`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Should named queries be checked during startup?
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.proc.param_null_passing`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Global setting for whether `null` parameter bindings should be passed to database procedure/function calls as part of [`ProcedureCall`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/procedure/ProcedureCall.html) handling.
    Implicitly Hibernate will not pass the `null`, the intention being to allow any default argument values to be applied.

    This defines a global setting, which can then be controlled per parameter via `org.hibernate.procedure.ParameterRegistration#enablePassingNulls(boolean)`

    Values are `true` (pass the NULLs) or `false` (do not pass the NULLs).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.log.warnings`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Enable fetching JDBC statement warning for logging. Default value is given by `org.hibernate.dialect.Dialect#isJdbcLogWarningsEnabledByDefault()`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.session_factory.statement_inspector`
    </td>
    <td class="tableblock halign-left valign-top">

    A fully-qualified class name, an instance, or a `Class` object reference
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Names a [`StatementInspector`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/resource/jdbc/spi/StatementInspector.html) implementation to be applied to every `Session` created by the current `SessionFactory`.

    </div>
    <div class="paragraph">

    Can reference

    </div>
    <div class="ulist">

*   `StatementInspector` instance
*   `StatementInspector` implementation {@link Class} reference
*   `StatementInspector` implementation class name (fully-qualified class name)
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Multi-table bulk HQL operations
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hql.bulk_id_strategy`
    </td>
    <td class="tableblock halign-left valign-top">

    A fully-qualified class name, an instance, or a `Class` object reference
    </td>
    <td class="tableblock halign-left valign-top">

    Provide a custom [`org.hibernate.hql.spi.id.MultiTableBulkIdStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/hql/spi/id/MultiTableBulkIdStrategy.html) implementation for handling multi-table bulk HQL operations.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hql.bulk_id_strategy.global_temporary.drop_tables`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    For databases that don&#8217;t support local tables, but just global ones, this configuration property allows you to DROP the global tables used for multi-table bulk HQL operations when the `SessionFactory` or the `EntityManagerFactory` is closed.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hql.bulk_id_strategy.persistent.drop_tables`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    This configuration property is used by the [`PersistentTableBulkIdStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/hql/spi/id/persistent/PersistentTableBulkIdStrategy.html), that mimics temporary tables for databases which do not support temporary tables.
    It follows a pattern similar to the ANSI SQL definition of global temporary table using a "session id" column to segment rows from the various sessions.

    This configuration property allows you to DROP the tables used for multi-table bulk HQL operations when the `SessionFactory` or the `EntityManagerFactory` is closed.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hql.bulk_id_strategy.persistent.schema`
    </td>
    <td class="tableblock halign-left valign-top">

    Database schema name. By default, the `hibernate.default_schema` is used.
    </td>
    <td class="tableblock halign-left valign-top">

    This configuration property is used by the [`PersistentTableBulkIdStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/hql/spi/id/persistent/PersistentTableBulkIdStrategy.html), that mimics temporary tables for databases which do not support temporary tables.
    It follows a pattern similar to the ANSI SQL definition of global temporary table using a "session id" column to segment rows from the various sessions.

    This configuration property defines the database schema used for storing the temporary tables used for bulk HQL operations.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hql.bulk_id_strategy.persistent.catalog`
    </td>
    <td class="tableblock halign-left valign-top">

    Database catalog name. By default, the `hibernate.default_catalog` is used.
    </td>
    <td class="tableblock halign-left valign-top">

    This configuration property is used by the [`PersistentTableBulkIdStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/hql/spi/id/persistent/PersistentTableBulkIdStrategy.html), that mimics temporary tables for databases which do not support temporary tables.
    It follows a pattern similar to the ANSI SQL definition of global temporary table using a "session id" column to segment rows from the various sessions.

    This configuration property defines the database catalog used for storing the temporary tables used for bulk HQL operations.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.legacy_limit_handler`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Setting which indicates whether or not to use `org.hibernate.dialect.pagination.LimitHandler`
    implementations that sacrifices performance optimizations to allow legacy 4.x limit behavior.

    Legacy 4.x behavior favored performing pagination in-memory by avoiding the use of the offset value, which is overall poor performance.
    In 5.x, the limit handler behavior favors performance, thus, if the dialect doesn&#8217;t support offsets, an exception is thrown instead.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">