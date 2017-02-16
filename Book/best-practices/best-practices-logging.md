 ### 25.2. Logging

    <div class="paragraph">

    Whenever you&#8217;re using a framework that generates SQL statements on your behalf, you have to ensure that the generated statements are the ones that you intended in the first place.

    </div>
    <div class="paragraph">

    There are several alternatives to logging statements.
    You can log statements by configuring the underlying logging framework.
    For Log4j, you can use the following appenders:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`### log just the SQL
    log4j.logger.org.hibernate.SQL=debug

    ### log JDBC bind parameters ###
    log4j.logger.org.hibernate.type=trace
    log4j.logger.org.hibernate.type.descriptor.sql=trace`</pre>
    </div>
    </div>
    <div class="paragraph">

    However, there are some other alternatives like using datasource-proxy or p6spy.
    The advantage of using a JDBC `Driver` or `DataSource` Proxy is that you can go beyond simple SQL logging:

    </div>
    <div class="ulist">

*   statement execution time
*   JDBC batching logging
*   [database connection monitoring](https://github.com/vladmihalcea/flexy-pool)
    </div>
    <div class="paragraph">

    Another advantage of using a `DataSource` proxy is that you can assert the number of executed statements at test time.
    This way, you can have the integration tests fail when a N+1 query issue is automatically detected.

    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    While simple statement logging is fine, using [datasource-proxy](https://github.com/ttddyy/datasource-proxy) or [p6spy](https://github.com/p6spy/p6spy) is even better.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">