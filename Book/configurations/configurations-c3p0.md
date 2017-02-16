 ### 23.4. c3p0 properties

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

    `hibernate.c3p0.min_size`
    </td>
    <td class="tableblock halign-left valign-top">

    1
    </td>
    <td class="tableblock halign-left valign-top">

    Minimum size of C3P0 connection pool. Refers to [c3p0 `minPoolSize` setting](http://www.mchange.com/projects/c3p0/#minPoolSize).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.c3p0.max_size`
    </td>
    <td class="tableblock halign-left valign-top">

    5
    </td>
    <td class="tableblock halign-left valign-top">

    Maximum size of C3P0 connection pool. Refers to [c3p0 `maxPoolSize` setting](http://www.mchange.com/projects/c3p0/#maxPoolSize).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.c3p0.timeout`
    </td>
    <td class="tableblock halign-left valign-top">

    30
    </td>
    <td class="tableblock halign-left valign-top">

    Maximum idle time for C3P0 connection pool. Refers to [c3p0 `maxIdleTime` setting](http://www.mchange.com/projects/c3p0/#maxIdleTime).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.c3p0.max_statements`
    </td>
    <td class="tableblock halign-left valign-top">

    5
    </td>
    <td class="tableblock halign-left valign-top">

    Maximum size of C3P0 statement cache. Refers to [c3p0 `maxStatements` setting](http://www.mchange.com/projects/c3p0/#maxStatements).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.c3p0.acquire_increment`
    </td>
    <td class="tableblock halign-left valign-top">

    2
    </td>
    <td class="tableblock halign-left valign-top">

    Number of connections acquired at a time when there&#8217;s no connection available in the pool. Refers to [c3p0 `acquireIncrement` setting](http://www.mchange.com/projects/c3p0/#acquireIncrement).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.c3p0.idle_test_period`
    </td>
    <td class="tableblock halign-left valign-top">

    5
    </td>
    <td class="tableblock halign-left valign-top">

    Idle time before a C3P0 pooled connection is validated. Refers to [c3p0 `idleConnectionTestPeriod` setting](http://www.mchange.com/projects/c3p0/#idleConnectionTestPeriod).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.c3p0`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    A setting prefix used to indicate additional c3p0 properties that need to be passed to the underlying c3p0 connection pool.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">