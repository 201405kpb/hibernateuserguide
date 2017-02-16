 ### 7.3. Using c3p0

    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    To use this integration, the application must include the hibernate-c3p0 module jar (as well as its dependencies) on the classpath.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate also provides support for applications to use [c3p0](http://www.mchange.com/projects/c3p0/) connection pooling.
    When using this c3p0 support, a number of additional configuration settings are recognized.

    </div>
    <div class="paragraph">

    Transaction isolation of the Connections is managed by the `ConnectionProvider` itself. See [ConnectionProvider support for transaction isolation setting](#database-connectionprovider-isolation).

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`hibernate.connection.driver_class`</dt>
    <dd>

    The name of the JDBC Driver class to use

    </dd>
    <dt class="hdlist1">`hibernate.connection.url`</dt>
    <dd>

    The JDBC connection url.

    </dd>
    <dt class="hdlist1">Any settings prefixed with `hibernate.connection.` (other than the "special ones")</dt>
    <dd>

    These all have the `hibernate.connection.` prefix stripped and the rest will be passed as JDBC connection properties

    </dd>
    <dt class="hdlist1">`hibernate.c3p0.min_size` or `c3p0.minPoolSize`</dt>
    <dd>

    The minimum size of the c3p0 pool. See [c3p0 minPoolSize](http://www.mchange.com/projects/c3p0/#minPoolSize)

    </dd>
    <dt class="hdlist1">`hibernate.c3p0.max_size` or `c3p0.maxPoolSize`</dt>
    <dd>

    The maximum size of the c3p0 pool. See [c3p0 maxPoolSize](http://www.mchange.com/projects/c3p0/#maxPoolSize)

    </dd>
    <dt class="hdlist1">`hibernate.c3p0.timeout` or `c3p0.maxIdleTime`</dt>
    <dd>

    The Connection idle time. See [c3p0 maxIdleTime](http://www.mchange.com/projects/c3p0/#maxIdleTime)

    </dd>
    <dt class="hdlist1">`hibernate.c3p0.max_statements` or `c3p0.maxStatements`</dt>
    <dd>

    Controls the c3p0 PreparedStatement cache size (if using). See [c3p0 maxStatements](http://www.mchange.com/projects/c3p0/#maxStatements)

    </dd>
    <dt class="hdlist1">`hibernate.c3p0.acquire_increment` or `c3p0.acquireIncrement`</dt>
    <dd>

    Number of connections c3p0 should acquire at a time when pool is exhausted. See [c3p0 acquireIncrement](http://www.mchange.com/projects/c3p0/#acquireIncrement)

    </dd>
    <dt class="hdlist1">`hibernate.c3p0.idle_test_period` or `c3p0.idleConnectionTestPeriod`</dt>
    <dd>

    Idle time before a c3p0 pooled connection is validated. See [c3p0 idleConnectionTestPeriod](http://www.mchange.com/projects/c3p0/#idleConnectionTestPeriod)

    </dd>
    <dt class="hdlist1">`hibernate.c3p0.initialPoolSize`</dt>
    <dd>

    The initial c3p0 pool size. If not specified, default is to use the min pool size. See [c3p0 initialPoolSize](http://www.mchange.com/projects/c3p0/#initialPoolSize)

    </dd>
    <dt class="hdlist1">Any other settings prefixed with `hibernate.c3p0.`</dt>
    <dd>

    Will have the `hibernate.` portion stripped and be passed to c3p0.

    </dd>
    <dt class="hdlist1">Any other settings prefixed with `c3p0.`</dt>
    <dd>

    Get passed to c3p0 as is. See [c3p0 configuration](http://www.mchange.com/projects/c3p0/#configuration)

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">
