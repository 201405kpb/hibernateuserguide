
    ### 7.2. Using DataSources

    <div class="paragraph">

    Hibernate can integrate with a `javax.sql.DataSource` for obtaining JDBC Connections.
    Applications would tell Hibernate about the `DataSource` via the (required) `hibernate.connection.datasource` setting which can either specify a JNDI name or would reference the actual `DataSource` instance.
    For cases where a JNDI name is given, be sure to read [JNDI](#jndi)

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    For JPA applications, note that `hibernate.connection.datasource` corresponds to either `javax.persistence.jtaDataSource` or `javax.persistence.nonJtaDataSource`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `DataSource` `ConnectionProvider` also (optionally) accepts the `hibernate.connection.username` and `hibernate.connection.password`.
    If specified, the [`DataSource#getConnection(String username, String password)`](https://docs.oracle.com/javase/8/docs/api/javax/sql/DataSource.html#getConnection-java.lang.String-java.lang.String-) will be used.
    Otherwise, the no-arg form is used.

    </div>
    </div>
    <div class="sect2">