### 7.11. ConnectionProvider support for transaction isolation setting

    <div class="paragraph">

    All of the provided ConnectionProvider implementations, other than `DataSourceConnectionProvider`, support consistent setting of transaction isolation for all `Connections` obtained from the underlying pool.
    The value for `hibernate.connection.isolation` can be specified in one of 3 formats:

    </div>
    <div class="ulist">

*   the integer value accepted at the JDBC level
*   the name of the `java.sql.Connection` constant field representing the isolation you would like to use.
    For example, `TRANSACTION_REPEATABLE_READ` for [`java.sql.Connection#TRANSACTION_REPEATABLE_READ`](https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html#TRANSACTION_REPEATABLE_READ).
    Not that this is only supported for JDBC standard isolation levels, not for isolation levels specific to a particular JDBC driver.
*   a short-name version of the java.sql.Connection constant field without the `TRANSACTION_` prefix. For example, `REPEATABLE_READ` for [`java.sql.Connection#TRANSACTION_REPEATABLE_READ`](https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html#TRANSACTION_REPEATABLE_READ).
    Again, this is only supported for JDBC standard isolation levels, not for isolation levels specific to a particular JDBC driver.
    </div>
    </div>
    <div class="sect2">