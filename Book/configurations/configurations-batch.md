 ### 23.8. Batching properties

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

    `hibernate.jdbc.batch_size`
    </td>
    <td class="tableblock halign-left valign-top">

    5
    </td>
    <td class="tableblock halign-left valign-top">

    Maximum JDBC batch size. A nonzero value enables batch updates.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.order_inserts`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Forces Hibernate to order SQL inserts by the primary key value of the items being inserted. This preserves batching when using cascading.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.order_updates`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Forces Hibernate to order SQL updates by the primary key value of the items being updated. This preserves batching when using cascading and reduces the likelihood of transaction deadlocks in highly-concurrent systems.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.batch_versioned_data`
    </td>
    <td class="tableblock halign-left valign-top">

    `true`(default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Should versioned entities be included in batching?

    Set this property to `true` if your JDBC driver returns correct row counts from executeBatch(). This option is usually safe, but is disabled by default. If enabled, Hibernate uses batched DML for automatically versioned data.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.batch_fetch_style`
    </td>
    <td class="tableblock halign-left valign-top">

    `LEGACY`(default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Names the [`BatchFetchStyle`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/loader/BatchFetchStyle.html) to use.

    Can specify either the [`BatchFetchStyle`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/loader/BatchFetchStyle.html) name (insensitively), or a [`BatchFetchStyle`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/loader/BatchFetchStyle.html) instance. `LEGACY}` is the default value.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.batch.builder`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of an [`BatchBuilder`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/batch/spi/BatchBuilder.html) implementation class type or an actual object instance
    </td>
    <td class="tableblock halign-left valign-top">

    Names the [`BatchBuilder`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/batch/spi/BatchBuilder.html) implementation to use.
    </td>
    </tr>
    </tbody>
    </table>
    <div class="sect3">