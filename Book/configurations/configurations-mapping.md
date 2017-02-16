 ### 23.5. Mapping Properties

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

    Table qualifying options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.default_catalog`
    </td>
    <td class="tableblock halign-left valign-top">

    A catalog name
    </td>
    <td class="tableblock halign-left valign-top">

    Qualifies unqualified table names with the given catalog in generated SQL.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.default_schema`
    </td>
    <td class="tableblock halign-left valign-top">

    A schema name
    </td>
    <td class="tableblock halign-left valign-top">

    Qualify unqualified table names with the given schema or tablespace in generated SQL.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.schema_name_resolver`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully qualified name of an [`org.hibernate.engine.jdbc.env.spi.SchemaNameResolver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/env/spi/SchemaNameResolver.html) implementation class
    </td>
    <td class="tableblock halign-left valign-top">

    By default, Hibernate uses the [`org.hibernate.dialect.Dialect#getSchemaNameResolver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/dialect/Dialect.html#getSchemaNameResolver--) You can customize how the schema name is resolved by providing a custom implementation of the [`SchemaNameResolver`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/engine/jdbc/env/spi/SchemaNameResolver.html) interface.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Identifier options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.id.new_generator_mappings`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Setting which indicates whether or not the new [`IdentifierGenerator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/id/IdentifierGenerator.html) are used for `AUTO`, `TABLE` and `SEQUENCE`.

    Existing applications may want to disable this (set it `false`) for upgrade compatibility from 3.x and 4.x to 5.x.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.use_identifier_rollback`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    If true, generated identifier properties are reset to default values when objects are deleted.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.id.optimizer.pooled.preferred`
    </td>
    <td class="tableblock halign-left valign-top">

    `none`, `hilo`, `legacy-hilo`, `pooled` (default value), `pooled-lo`, `pooled-lotl` or a fully-qualified name of the [`Optimizer`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/id/enhanced/Optimizer.html) implementation
    </td>
    <td class="tableblock halign-left valign-top">

    When a generator specified an increment-size and an optimizer was not explicitly specified, which of the _pooled_ optimizers should be preferred?
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Quoting options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.globally_quoted_identifiers`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Should all database identifiers be quoted.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.globally_quoted_identifiers_skip_column_definitions`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Assuming `hibernate.globally_quoted_identifiers` is `true`, this allows the global quoting to skip column-definitions as defined by `javax.persistence.Column`,
    `javax.persistence.JoinColumn`, etc, and while it avoids column-definitions being quoted due to global quoting, they can still be explicitly quoted in the annotation/xml mappings.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.auto_quote_keyword`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Specifies whether to automatically quote any names that are deemed keywords.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Discriminator options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.discriminator.implicit_for_joined`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    The legacy behavior of Hibernate is to not use discriminators for joined inheritance (Hibernate does not need the discriminator).
    However, some JPA providers do need the discriminator for handling joined inheritance so, in the interest of portability, this capability has been added to Hibernate too.

    However, we want to make sure that legacy applications continue to work as well, which puts us in a bind in terms of how to handle _implicit_ discriminator mappings.
    The solution is to assume that the absence of discriminator metadata means to follow the legacy behavior _unless_ this setting is enabled.

    With this setting enabled, Hibernate will interpret the absence of discriminator metadata as an indication to use the JPA-defined defaults for these absent annotations.

    See Hibernate Jira issue [HHH-6911](https://hibernate.atlassian.net/browse/HHH-6911) for additional background info.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.discriminator.ignore_explicit_for_joined`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    The legacy behavior of Hibernate is to not use discriminators for joined inheritance (Hibernate does not need the discriminator).
    However, some JPA providers do need the discriminator for handling joined inheritance so, in the interest of portability, this capability has been added to Hibernate too.

    Existing applications rely (implicitly or explicitly) on Hibernate ignoring any `DiscriminatorColumn` declarations on joined inheritance hierarchies.
    This setting allows these applications to maintain the legacy behavior of `DiscriminatorColumn` annotations being ignored when paired with joined inheritance.

    See Hibernate Jira issue [HHH-6911](https://hibernate.atlassian.net/browse/HHH-6911) for additional background info.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Naming strategies
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.implicit_naming_strategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `default` (default value), `jpa`, `legacy-jpa`, `legacy-hbm`, `component-path`
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Used to specify the [`ImplicitNamingStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/naming/ImplicitNamingStrategy.html) class to use.
    The following short names are defined for this setting:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`default`</dt>
    <dd>

    Uses the [`ImplicitNamingStrategyJpaCompliantImpl`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/naming/ImplicitNamingStrategyJpaCompliantImpl.html)

    </dd>
    <dt class="hdlist1">`jpa`</dt>
    <dd>

    Uses the [`ImplicitNamingStrategyJpaCompliantImpl`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/naming/ImplicitNamingStrategyJpaCompliantImpl.html)

    </dd>
    <dt class="hdlist1">`legacy-jpa`</dt>
    <dd>

    Uses the [`ImplicitNamingStrategyLegacyJpaImpl`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/naming/ImplicitNamingStrategyLegacyJpaImpl.html)

    </dd>
    <dt class="hdlist1">`legacy-hbm`</dt>
    <dd>

    Uses the [`ImplicitNamingStrategyLegacyHbmImpl`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/naming/ImplicitNamingStrategyLegacyHbmImpl.html)

    </dd>
    <dt class="hdlist1">`component-path`</dt>
    <dd>

    Uses the [`ImplicitNamingStrategyComponentPathImpl`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/naming/ImplicitNamingStrategyComponentPathImpl.html)

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    If this property happens to be empty, the fallback is to use `default` strategy.

    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.physical_naming_strategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.boot.model.naming.
    PhysicalNamingStrategyStandardImpl` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Used to specify the [`PhysicalNamingStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/model/naming/PhysicalNamingStrategy.html) class to use.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Metadata scanning options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.archive.scanner`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Pass an implementation of [`Scanner`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/archive/scan/spi/Scanner.html).
    By default, [`StandardScanner`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/archive/scan/internal/StandardScanner.html) is used.

    </div>
    <div class="paragraph">

    Accepts either:

    </div>
    <div class="ulist">

*   an actual `Scanner` instance
*   a reference to a Class that implements `Scanner`
*   a fully qualified name of a Class that implements `Scanner`
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.archive.interpreter`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Pass [`ArchiveDescriptorFactory`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/archive/spi/ArchiveDescriptorFactory.html) to use in the scanning process.

    </div>
    <div class="paragraph">

    Accepts either:

    </div>
    <div class="ulist">

*   an actual `ArchiveDescriptorFactory` instance
*   a reference to a Class that implements `ArchiveDescriptorFactory`
*   a fully qualified name of a Class that implements `ArchiveDescriptorFactory`
    </div>
    <div class="paragraph">

    See information on [`Scanner`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/boot/archive/scan/spi/Scanner.html) about expected constructor forms.

    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.archive.autodetection`
    </td>
    <td class="tableblock halign-left valign-top">

    `hbm,class` (default value)
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Identifies a comma-separate list of values indicating the mapping types we should auto-detect during scanning.

    </div>
    <div class="paragraph">

    Allowable values include:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`class`</dt>
    <dd>

    scan classes (e.g. `.class`) to extract entity mapping metadata

    </dd>
    <dt class="hdlist1">`hbm`</dt>
    <dd>

    scan `hbm` mapping files (e.g. `hbm.xml`) to extract entity mapping metadata

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    By default both HBM, annotations, and JPA XML mappings are scanned.

    </div>
    <div class="paragraph">

    When using JPA, to disable the automatic scanning of all entity classes, the `exclude-unlisted-classes` `persistence.xml` element must be set to true.
    Therefore, when setting `exclude-unlisted-classes` to true, only the classes that are explicitly declared in the `persistence.xml` configuration files are going to be taken into consideration.

    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.mapping.precedence`
    </td>
    <td class="tableblock halign-left valign-top">

    `hbm,class` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Used to specify the order in which metadata sources should be processed.
    Value is a delimited-list whose elements are defined by [`MetadataSourceType`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/cfg/MetadataSourceType.html).

    Default is `hbm,class"`, therefore `hbm.xml` files are processed first, followed by annotations (combined with `orm.xml` mappings).

    When using JPA, the XML mapping overrides a conflicting annotation mapping that targets the same entity attribute.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    JDBC-related options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.use_nationalized_character_data`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable nationalized character support on all string / clob based attribute ( string, char, clob, text etc ).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.lob.non_contextual_creation`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Should we not use contextual LOB creation (aka based on `java.sql.Connection#createBlob()` et al)? The default value for HANA, H2, and PostgreSQL is `true`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.jdbc.time_zone`
    </td>
    <td class="tableblock halign-left valign-top">

    A `java.util.TimeZone`, a `java.time.ZoneId` or a `String` representation of a `ZoneId`
    </td>
    <td class="tableblock halign-left valign-top">

    Unless specified, the JDBC Driver uses the default JVM time zone. If a different time zone is configured via this setting, the JDBC [PreparedStatement#setTimestamp](https://docs.oracle.com/javase/8/docs/api/java/sql/PreparedStatement.html#setTimestamp-int-java.sql.Timestamp-java.util.Calendar-) is going to use a `Calendar` instance according to the specified time zone.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.dialect.oracle.prefer_long_raw`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    This setting applies to Oracle Dialect only, and it specifies whether `byte[]` or `Byte[]` arrays should be mapped to the deprecated `LONG RAW` (when this configuration property value is `true`) or to a `BLOB` column type (when this configuration property value is `false`).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Bean Validation options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.validation.factory`
    </td>
    <td class="tableblock halign-left valign-top">

    `javax.validation.ValidationFactory` implementation
    </td>
    <td class="tableblock halign-left valign-top">

    Specify the  `javax.validation.ValidationFactory` implementation to use for Bean Validation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.check_nullability`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Enable nullability checking. Raises an exception if a property marked as not-null is null.

    Default to `false` if Bean Validation is present in the classpath and Hibernate Annotations is used, `true` otherwise.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.validator.apply_to_ddl`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Bean Validation constraints will be applied in DDL if the automatic schema generation is enabled.
    In other words, the database schema will reflect the Bean Validation constraints.

    To disable constraint propagation to DDL, set up `hibernate.validator.apply_to_ddl` to `false` in the configuration file.
    Such a need is very uncommon and not recommended.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top" colspan="3">

    Misc options
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.create_empty_composites.enabled`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enable instantiation of composite/embeddable objects when all of its attribute values are `null`. The default (and historical) behavior is that a `null` reference will be used to represent the composite when all of its attributes are `null`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.entity_dirtiness_strategy`
    </td>
    <td class="tableblock halign-left valign-top">

    fully-qualified class name or an actual `CustomEntityDirtinessStrategy` instance
    </td>
    <td class="tableblock halign-left valign-top">

    Setting to identify a `org.hibernate.CustomEntityDirtinessStrategy` to use.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.default_entity_mode`
    </td>
    <td class="tableblock halign-left valign-top">

    `pojo` (default value) or `dynamic-map`
    </td>
    <td class="tableblock halign-left valign-top">

    Default `EntityMode` for entity representation for all sessions opened from this `SessionFactory`, defaults to `pojo`.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">