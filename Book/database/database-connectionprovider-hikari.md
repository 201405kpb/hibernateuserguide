 ### 7.8. Using Hikari

    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    To use this integration, the application must include the hibernate-hikari module jar (as well as its dependencies) on the classpath.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate also provides support for applications to use [Hikari](http://brettwooldridge.github.io/HikariCP/) connection pool.

    </div>
    <div class="paragraph">

    Set all of your Hikari settings in Hibernate prefixed by `hibernate.hikari.` and this `ConnectionProvider` will pick them up and pass them along to Hikari.
    Additionally, this `ConnectionProvider` will pick up the following Hibernate-specific properties and map them to the corresponding Hikari ones (any `hibernate.hikari.` prefixed ones have precedence):

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`hibernate.connection.driver_class`</dt>
    <dd>

    Mapped to Hikari&#8217;s `driverClassName` setting

    </dd>
    <dt class="hdlist1">`hibernate.connection.url`</dt>
    <dd>

    Mapped to Hikari&#8217;s `jdbcUrl` setting

    </dd>
    <dt class="hdlist1">`hibernate.connection.username`</dt>
    <dd>

    Mapped to Hikari&#8217;s `username` setting

    </dd>
    <dt class="hdlist1">`hibernate.connection.password`</dt>
    <dd>

    Mapped to Hikari&#8217;s `password` setting

    </dd>
    <dt class="hdlist1">`hibernate.connection.isolation`</dt>
    <dd>

    Mapped to Hikari&#8217;s `transactionIsolation` setting. See [ConnectionProvider support for transaction isolation setting](#database-connectionprovider-isolation).
    Note that Hikari only supports JDBC standard isolation levels (apparently).

    </dd>
    <dt class="hdlist1">`hibernate.connection.autocommit`</dt>
    <dd>

    Mapped to Hikari&#8217;s `autoCommit` setting

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">