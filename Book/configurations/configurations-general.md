 ### 23.2. General Configuration

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

    `hibernate.dialect`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.dialect.
    PostgreSQL94Dialect`
    </td>
    <td class="tableblock halign-left valign-top">

    The classname of a Hibernate [`Dialect`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/dialect/Dialect.html) from which Hibernate can generate SQL optimized for a particular relational database.

    In most cases Hibernate can choose the correct [`Dialect`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/dialect/Dialect.html) implementation based on the JDBC metadata returned by the JDBC driver.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.current_session_context_class`
    </td>
    <td class="tableblock halign-left valign-top">

    `jta`, `thread`, `managed`, or a custom class implementing `org.hibernate.context.spi.
    CurrentSessionContext`
    </td>
    <td class="tableblock halign-left valign-top">

    Supply a custom strategy for the scoping of the _current_ `Session`.

    The definition of what exactly _current_ means is controlled by the [`CurrentSessionContext`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/spi/CurrentSessionContext.html) implementation in use.

    Note that for backwards compatibility, if a [`CurrentSessionContext`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/spi/CurrentSessionContext.html) is not configured but JTA is configured this will default to the [`JTASessionContext`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/context/internal/JTASessionContext.html).
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">