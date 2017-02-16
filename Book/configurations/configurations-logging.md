### 23.9. Statement logging and statistics

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
    <td class="tableblock halign-left valign-top" colspan="3">

    SQL statement logging
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.show_sql`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Write all SQL statements to the console. This is an alternative to setting the log category `org.hibernate.SQL` to debug.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.format_sql`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Pretty-print the SQL in the log and console.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.use_sql_comments`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    If true, Hibernate generates comments inside the SQL, for easier debugging.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Statistics settings
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.generate_statistics`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Causes Hibernate to collect statistics for performance tuning.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.stats.factory`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of an [`StatisticsFactory`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/stat/spi/StatisticsFactory.html) implementation or an actual instance
    The `StatisticsFactory` allow you to customize how the Hibernate Statistics are being collected.
    </td>
    <td class="tableblock halign-left valign-top">

    `hibernate.session.events.log`
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">