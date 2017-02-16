### 23.3. Database connection properties

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

    `hibernate.connection.driver_class` or `javax.persistence.jdbc.driver`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.postgresql.Driver`
    </td>
    <td class="tableblock halign-left valign-top">

    Names the JDBC `Driver` class name.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.url` or `javax.persistence.jdbc.url`
    </td>
    <td class="tableblock halign-left valign-top">

    `jdbc:postgresql:hibernate_orm_test`
    </td>
    <td class="tableblock halign-left valign-top">

    Names the JDBC connection URL.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.username` or `javax.persistence.jdbc.user`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the JDBC connection user name.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.password` or `javax.persistence.jdbc.password`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the JDBC connection password.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.isolation`
    </td>
    <td class="tableblock halign-left valign-top">

    `REPEATABLE_READ` or
    `Connection.TRANSACTION_REPEATABLE_READ`
    </td>
    <td class="tableblock halign-left valign-top">

    Names the JDBC connection transaction isolation level.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.autocommit`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Names the JDBC connection autocommit mode.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.datasource`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Either a `javax.sql.DataSource` instance or a JNDI name under which to locate the `DataSource`.

    For JNDI names, ses also `hibernate.jndi.class`, `hibernate.jndi.url`, `hibernate.jndi`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names a prefix used to define arbitrary JDBC connection properties. These properties are passed along to the JDBC provider when creating a connection.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.provider_class`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.hikaricp.internal.
    HikariCPConnectionProvider`
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Names the [`ConnectionProvider`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/connections/spi/ConnectionProvider.html) to use for obtaining JDBC connections.

    </div>
    <div class="paragraph">

    Can reference:

    </div>
    <div class="ulist">

*   an instance of `ConnectionProvider`
*   a `Class&lt;? extends ConnectionProvider` object reference
*   a fully qualified name of a class implementing `ConnectionProvider`
    </div>
    <div class="paragraph">

    The term `class` appears in the setting name due to legacy reasons; however it can accept instances.

    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jndi.class`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names the JNDI `javax.naming.InitialContext` class.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jndi.url`
    </td>
    <td class="tableblock halign-left valign-top">

    java:global/jdbc/default
    </td>
    <td class="tableblock halign-left valign-top">

    Names the JNDI provider/connection url.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jndi`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Names a prefix used to define arbitrary JNDI `javax.naming.InitialContext` properties.

    These properties are passed along to `javax.naming.InitialContext#InitialContext(java.util.Hashtable)`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.acquisition_mode`
    </td>
    <td class="tableblock halign-left valign-top">

    `immediate`
    </td>
    <td class="tableblock halign-left valign-top">

    Specifies how Hibernate should acquire JDBC connections. The possible values are given by `org.hibernate.ConnectionAcquisitionMode`.

    Should generally only configure this or `hibernate.connection.release_mode`, not both.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.release_mode`
    </td>
    <td class="tableblock halign-left valign-top">

    `auto` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Specifies how Hibernate should release JDBC connections. The possible values are given by the current transaction mode (`after_transaction` for JDBC transactions and `after_statement` for JTA transactions).

    Should generally only configure this or `hibernate.connection.acquisition_mode`, not both.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Hibernate internal connection pool options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.initial_pool_size`
    </td>
    <td class="tableblock halign-left valign-top">

    1 (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Minimum number of connections for the built-in Hibernate connection pool.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.pool_size`
    </td>
    <td class="tableblock halign-left valign-top">

    20 (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Maximum number of connections for the built-in Hibernate connection pool.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.connection.pool_validation_interval`
    </td>
    <td class="tableblock halign-left valign-top">

    30 (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    The number of seconds between two consecutive pool validations. During validation, the pool size can increase or decreases based on the connection acquisition request count.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">