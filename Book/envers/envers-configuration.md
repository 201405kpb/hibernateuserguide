 ### 21.2. Configuration

    <div class="paragraph">

    It is possible to configure various aspects of Hibernate Envers behavior, such as table names, etc.

    </div>
    <table class="tableblock frame-all grid-all spread">
    <caption class="title">Table 10. Envers Configuration Properties</caption>
    <colgroup>
    <col style="width: 34%;">
    <col style="width: 33%;">
    <col style="width: 33%;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Property name</th>
    <th class="tableblock halign-left valign-top">Default value</th>
    <th class="tableblock halign-left valign-top">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.audit_table_prefix`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    String that will be prepended to the name of an audited entity to create the name of the entity and that will hold audit information.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.audit_table_suffix`
    </td>
    <td class="tableblock halign-left valign-top">

    `_AUD`
    </td>
    <td class="tableblock halign-left valign-top">

    String that will be appended to the name of an audited entity to create the name of the entity and that will hold audit information.
      If you audit an entity with a table name Person, in the default setting Envers will generate a `Person_AUD` table to store historical data.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.revision_field_name`
    </td>
    <td class="tableblock halign-left valign-top">

    `REV`
    </td>
    <td class="tableblock halign-left valign-top">

    Name of a field in the audit entity that will hold the revision number.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.revision_type_field_name`
    </td>
    <td class="tableblock halign-left valign-top">

    `REVTYPE`
    </td>
    <td class="tableblock halign-left valign-top">

    Name of a field in the audit entity that will hold the type of the revision (currently, this can be: `add`, `mod`, `del`).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.revision_on_collection_change`
    </td>
    <td class="tableblock halign-left valign-top">

    `true`
    </td>
    <td class="tableblock halign-left valign-top">

    Should a revision be generated when a not-owned relation field changes (this can be either a collection in a one-to-many relation, or the field using `mappedBy` attribute in a one-to-one relation).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.do_not_audit_optimistic_locking_field`
    </td>
    <td class="tableblock halign-left valign-top">

    `true`
    </td>
    <td class="tableblock halign-left valign-top">

    When true, properties to be used for optimistic locking, annotated with `@Version`, will not be automatically audited (their history won&#8217;t be stored; it normally doesn&#8217;t make sense to store it).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.store_data_at_delete`
    </td>
    <td class="tableblock halign-left valign-top">

    `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Should the entity data be stored in the revision when the entity is deleted (instead of only storing the id and all other properties as null).
      This is not normally needed, as the data is present in the last-but-one revision.
      Sometimes, however, it is easier and more efficient to access it in the last revision (then the data that the entity contained before deletion is stored twice).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.default_schema`
    </td>
    <td class="tableblock halign-left valign-top">

    `null` (same schema as table being audited)
    </td>
    <td class="tableblock halign-left valign-top">

    The default schema name that should be used for audit tables.
      Can be overridden using the `@AuditTable( schema="&#8230;&#8203;" )` annotation.
      If not present, the schema will be the same as the schema of the table being audited.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.default_catalog`
    </td>
    <td class="tableblock halign-left valign-top">

    `null` (same catalog as table being audited)
    </td>
    <td class="tableblock halign-left valign-top">

    The default catalog name that should be used for audit tables.
      Can be overridden using the `@AuditTable( catalog="&#8230;&#8203;" )` annotation. If not present, the catalog will be the same as the catalog of the normal tables.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.audit_strategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.strategy.DefaultAuditStrategy`
    </td>
    <td class="tableblock halign-left valign-top">

    The audit strategy that should be used when persisting audit data.
      The default stores only the revision, at which an entity was modified.
      An alternative, the `org.hibernate.envers.strategy.ValidityAuditStrategy` stores both the start revision and the end revision.
      Together these define when an audit row was valid, hence the name ValidityAuditStrategy.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.audit_strategy_validity_end_rev_field_name`
    </td>
    <td class="tableblock halign-left valign-top">

    `REVEND`
    </td>
    <td class="tableblock halign-left valign-top">

    The column name that will hold the end revision number in audit entities.
      This property is only valid if the validity audit strategy is used.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.audit_strategy_validity_store_revend_timestamp`
    </td>
    <td class="tableblock halign-left valign-top">

    `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Should the timestamp of the end revision be stored, until which the data was valid, in addition to the end revision itself.
      This is useful to be able to purge old Audit records out of a relational database by using table partitioning.
      Partitioning requires a column that exists within the table.
      This property is only evaluated if the `ValidityAuditStrategy` is used.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.audit_strategy_validity_revend_timestamp_field_name`
    </td>
    <td class="tableblock halign-left valign-top">

    `REVEND_TSTMP`
    </td>
    <td class="tableblock halign-left valign-top">

    Column name of the timestamp of the end revision until which the data was valid.
      Only used if the 1ValidityAuditStrategy1 is used, and `org.hibernate.envers.audit_strategy_validity_store_revend_timestamp` evaluates to true
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.use_revision_entity_with_native_id`
    </td>
    <td class="tableblock halign-left valign-top">

    `true`
    </td>
    <td class="tableblock halign-left valign-top">

    Boolean flag that determines the strategy of revision number generation.
      Default implementation of revision entity uses native identifier generator.
      If current database engine does not support identity columns, users are advised to set this property to false.
      In this case revision numbers are created by preconfigured `org.hibernate.id.enhanced.SequenceStyleGenerator`.
      See: `org.hibernate.envers.DefaultRevisionEntity` and `org.hibernate.envers.enhanced.SequenceIdRevisionEntity`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.track_entities_changed_in_revision`
    </td>
    <td class="tableblock halign-left valign-top">

    `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Should entity types, that have been modified during each revision, be tracked.
      The default implementation creates `REVCHANGES` table that stores entity names of modified persistent objects.
      Single record encapsulates the revision identifier (foreign key to `REVINFO` table) and a string value.
      For more information, refer to [Tracking entity names modified during revisions](#envers-tracking-modified-entities-revchanges) and [Querying for entities modified in a given revision](#envers-tracking-modified-entities-queries).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.global_with_modified_flag`
    </td>
    <td class="tableblock halign-left valign-top">

    `false`, can be individually overridden with `@Audited( withModifiedFlag=true )`
    </td>
    <td class="tableblock halign-left valign-top">

    Should property modification flags be stored for all audited entities and all properties.
      When set to true, for all properties an additional boolean column in the audit tables will be created, filled with information if the given property changed in the given revision.
      When set to false, such column can be added to selected entities or properties using the `@Audited` annotation.
      For more information, refer to [Tracking entity changes at property level](#envers-tracking-properties-changes) and [Querying for revisions of entity that modified given property](#envers-tracking-properties-changes-queries).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.modified_flag_suffix`
    </td>
    <td class="tableblock halign-left valign-top">

    `_MOD`
    </td>
    <td class="tableblock halign-left valign-top">

    The suffix for columns storing "Modified Flags".
      For example: a property called "age", will by default get modified flag with column name "age_MOD".
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.embeddable_set_ordinal_field_name`
    </td>
    <td class="tableblock halign-left valign-top">

    `SETORDINAL`
    </td>
    <td class="tableblock halign-left valign-top">

    Name of column used for storing ordinal of the change in sets of embeddable elements.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.cascade_delete_revision`
    </td>
    <td class="tableblock halign-left valign-top">

    `false`
    </td>
    <td class="tableblock halign-left valign-top">

    While deleting revision entry, remove data of associated audited entities. Requires database support for cascade row removal.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.envers.allow_identifier_reuse`
    </td>
    <td class="tableblock halign-left valign-top">

    `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Guarantees proper validity audit strategy behavior when application reuses identifiers of deleted entities. Exactly one row with `null` end date exists for each identifier.
    </td>
    </tr>
    </tbody>
    </table>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The following configuration options have been added recently and should
    be regarded as experimental:

    </div>
    <div class="olist arabic">

1.  `org.hibernate.envers.track_entities_changed_in_revision`
2.  `org.hibernate.envers.using_modified_flag`
3.  `org.hibernate.envers.modified_flag_suffix`
    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">