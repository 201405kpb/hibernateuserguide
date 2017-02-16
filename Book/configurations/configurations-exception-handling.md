### 23.15. Exception handling

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

    `hibernate.jdbc.sql_exception_converter`
    </td>
    <td class="tableblock halign-left valign-top">

    Fully-qualified name of class implementing `SQLExceptionConverter`
    </td>
    <td class="tableblock halign-left valign-top">

    The [`SQLExceptionConverter`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/exception/spi/SQLExceptionConverter.html) to use for converting `SQLExceptions` to Hibernate&#8217;s `JDBCException` hierarchy. The default is to use the configured [`Dialect`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/dialect/Dialect.html)'s preferred `SQLExceptionConverter`.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">
