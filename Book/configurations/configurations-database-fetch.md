#### 23.8.1. Fetching properties

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

    `hibernate.max_fetch_depth`
    </td>
    <td class="tableblock halign-left valign-top">

    A value between `0` and `3`
    </td>
    <td class="tableblock halign-left valign-top">

    Sets a maximum depth for the outer join fetch tree for single-ended associations. A single-ended association is a one-to-one or many-to-one assocation. A value of `0` disables default outer join fetching.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.default_batch_fetch_size`
    </td>
    <td class="tableblock halign-left valign-top">

    `4`,`8`, or `16`
    </td>
    <td class="tableblock halign-left valign-top">

    Default size for Hibernate Batch fetching of associations (lazily fetched associations can be fetched in batches to prevent N+1 query problems).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.fetch_size`
    </td>
    <td class="tableblock halign-left valign-top">

    `0` or an integer
    </td>
    <td class="tableblock halign-left valign-top">

    A non-zero value determines the JDBC fetch size, by calling `Statement.setFetchSize()`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.use_scrollable_resultset`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Enables Hibernate to use JDBC2 scrollable resultsets. This property is only relevant for user-supplied JDBC connections. Otherwise, Hibernate uses connection metadata.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.use_streams_for_binary`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Use streams when writing or reading `binary` or `serializable` types to or from JDBC. This is a system-level property.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.use_get_generated_keys`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Allows Hibernate to use JDBC3 `PreparedStatement.getGeneratedKeys()` to retrieve natively-generated keys after insert. You need the JDBC3+ driver and JRE1.4+. Disable this property if your driver has problems with the Hibernate identifier generators. By default, it tries to detect the driver capabilities from connection metadata.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.wrap_result_sets`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable wrapping of JDBC result sets in order to speed up column name lookups for broken JDBC drivers.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.enable_lazy_load_no_trans`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Initialize Lazy Proxies or Collections outside a given Transactional Persistence Context.

    Although enabling this configuration can make `LazyInitializationException` go away, it&#8217;s better to use a fetch plan that guarantees that all properties are properly initialised before the Session is closed.

    In reality, you shouldn&#8217;t probably enable this setting anyway.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    </div>
    <div class="sect2">