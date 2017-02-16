 ### 12.1. JDBC batching

    <div class="paragraph">

    JDBC offers support for batching together SQL statements that can be represented as a single PreparedStatement.
    Implementation wise this generally means that drivers will send the batched operation to the server in one call,
    which can save on network calls to the database. Hibernate can leverage JDBC batching.
    The following settings control this behavior.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`hibernate.jdbc.batch_size`</dt>
    <dd>

    Controls the maximum number of statements Hibernate will batch together before asking the driver to execute the batch.
    Zero or a negative number disables this feature.

    </dd>
    <dt class="hdlist1">`hibernate.jdbc.batch_versioned_data`</dt>
    <dd>

    Some JDBC drivers return incorrect row counts when a batch is executed.
    If your JDBC driver falls into this category this setting should be set to `false`.
    Otherwise, it is safe to enable this which will allow Hibernate to still batch the DML for versioned entities and still use the returned row counts for optimistic lock checks.
    Since 5.0, it defaults to true. Previously (versions 3.x and 4.x), it used to be false.

    </dd>
    <dt class="hdlist1">`hibernate.jdbc.batch.builder`</dt>
    <dd>

    Names the implementation class used to manage batching capabilities.
    It is almost never a good idea to switch from Hibernate&#8217;s default implementation.
    But if you wish to, this setting would name the `org.hibernate.engine.jdbc.batch.spi.BatchBuilder` implementation to use.

    </dd>
    <dt class="hdlist1">`hibernate.order_updates`</dt>
    <dd>

    Forces Hibernate to order SQL updates by the entity type and the primary key value of the items being updated.
    This allows for more batching to be used. It will also result in fewer transaction deadlocks in highly concurrent systems.
    Comes with a performance hit, so benchmark before and after to see if this actually helps or hurts your application.

    </dd>
    <dt class="hdlist1">`hibernate.order_inserts`</dt>
    <dd>

    Forces Hibernate to order inserts to allow for more batching to be used.
    Comes with a performance hit, so benchmark before and after to see if this actually helps or hurts your application.

    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Since version 5.2, Hibernate allows overriding the global JDBC batch size given by the `hibernate.jdbc.batch_size` configuration property for a given `Session`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="batch-session-jdbc-batch-size-example" class="exampleblock">
    <div class="title">Example 298. Hibernate specific JDBC batch size configuration on a per `Session` basis</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`entityManager
        .unwrap( Session.class )
        .setJdbcBatchSize( 10 );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">