  ### 21.21. Suitable columns for audit table partitioning

    <div class="paragraph">

    Generally, SQL tables must be partitioned on a column that exists within the table.
    As a rule it makes sense to use either the _end revision_ or the _end revision timestamp_ column for partitioning of audit tables.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    End revision information is not available for the default `AuditStrategy`.

    </div>
    <div class="paragraph">

    Therefore the following Envers configuration options are required:

    </div>
    <div class="paragraph">

    `org.hibernate.envers.audit_strategy` = `org.hibernate.envers.strategy.ValidityAuditStrategy`

    </div>
    <div class="paragraph">

    `org.hibernate.envers.audit_strategy_validity_store_revend_timestamp` = `true`

    </div>
    <div class="paragraph">

    Optionally, you can also override the default values using following properties:

    </div>
    <div class="paragraph">

    `org.hibernate.envers.audit_strategy_validity_end_rev_field_name`

    </div>
    <div class="paragraph">

    `org.hibernate.envers.audit_strategy_validity_revend_timestamp_field_name`

    </div>
    <div class="paragraph">

    For more information, see [Configuration](#envers-configuration).

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The reason why the end revision information should be used for audit table partitioning is based on the assumption that audit tables should be partitioned on an 'increasing level of relevancy', like so:

    </div>
    <div class="olist arabic">

1.  A couple of partitions with audit data that is not very (or no longer) relevant.
    This can be stored on slow media, and perhaps even be purged eventually.
2.  Some partitions for audit data that is potentially relevant.
3.  One partition for audit data that is most likely to be relevant.
    This should be stored on the fastest media, both for reading and writing.
    </div>
    </div>
    <div class="sect2">