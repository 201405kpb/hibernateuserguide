### 23.14. Automatic schema generation

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

    `hibernate.hbm2ddl.auto`
    </td>
    <td class="tableblock halign-left valign-top">

    `none` (default value), `create-only`, `drop`, `create`, `create-drop`, `validate`, and `update`
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting to perform `SchemaManagementTool` actions automatically as part of the `SessionFactory` lifecycle.
    Valid options are defined by the `externalHbm2ddlName` value of the [`Action`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/Action.html) enum:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`none`</dt>
    <dd>

    No action will be performed.

    </dd>
    <dt class="hdlist1">`create-only`</dt>
    <dd>

    Database creation will be generated.

    </dd>
    <dt class="hdlist1">`drop`</dt>
    <dd>

    Database dropping will be generated.

    </dd>
    <dt class="hdlist1">`create`</dt>
    <dd>

    Database dropping will be generated followed by database creation.

    </dd>
    <dt class="hdlist1">`create-drop`</dt>
    <dd>

    Drop the schema and recreate it on SessionFactory startup.  Additionally, drop the schema on SessionFactory shutdown.

    </dd>
    <dt class="hdlist1">`validate`</dt>
    <dd>

    Validate the database schema

    </dd>
    <dt class="hdlist1">`update`</dt>
    <dd>

    Update the database schema

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.database.action`
    </td>
    <td class="tableblock halign-left valign-top">

    `none` (default value), `create-only`, `drop`, `create`, `create-drop`, `validate`, and `update`
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting to perform `SchemaManagementTool` actions automatically as part of the `SessionFactory` lifecycle.
    Valid options are defined by the `externalJpaName` value of the [`Action`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/Action.html) enum:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`none`</dt>
    <dd>

    No action will be performed.

    </dd>
    <dt class="hdlist1">`create`</dt>
    <dd>

    Database creation will be generated.

    </dd>
    <dt class="hdlist1">`drop`</dt>
    <dd>

    Database dropping will be generated.

    </dd>
    <dt class="hdlist1">`drop-and-create`</dt>
    <dd>

    Database dropping will be generated followed by database creation.

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.scripts.action`
    </td>
    <td class="tableblock halign-left valign-top">

    `none` (default value), `create-only`, `drop`, `create`, `create-drop`, `validate`, and `update`
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting to perform `SchemaManagementTool` actions writing the commands into a DDL script file.
    Valid options are defined by the `externalJpaName` value of the [`Action`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/Action.html) enum:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`none`</dt>
    <dd>

    No action will be performed.

    </dd>
    <dt class="hdlist1">`create`</dt>
    <dd>

    Database creation will be generated.

    </dd>
    <dt class="hdlist1">`drop`</dt>
    <dd>

    Database dropping will be generated.

    </dd>
    <dt class="hdlist1">`drop-and-create`</dt>
    <dd>

    Database dropping will be generated followed by database creation.

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation-connection`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Allows passing a specific `java.sql.Connection` instance to be used by `SchemaManagementTool`
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.database-product-name`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Specifies the name of the database provider in cases where a Connection to the underlying database is not available (aka, mainly in generating scripts).
    In such cases, a value for this setting _must_ be specified.

    The value of this setting is expected to match the value returned by `java.sql.DatabaseMetaData#getDatabaseProductName()` for the target database.

    Additionally, specifying `javax.persistence.database-major-version` and/or `javax.persistence.database-minor-version` may be required to understand exactly how to generate the required schema commands.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.database-major-version`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Specifies the major version of the underlying database, as would be returned by `java.sql.DatabaseMetaData#getDatabaseMajorVersion` for the target database.

    This value is used to help more precisely determine how to perform schema generation tasks for the underlying database in cases where `javax.persistence.database-product-name` does not provide enough distinction.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.database-minor-version`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Specifies the minor version of the underlying database, as would be returned by `java.sql.DatabaseMetaData#getDatabaseMinorVersion` for the target database.

    This value is used to help more precisely determine how to perform schema generation tasks for the underlying database in cases where `javax.persistence.database-product-name` and `javax.persistence.database-major-version` does not provide enough distinction.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.create-source`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Specifies whether schema generation commands for schema creation are to be determine based on object/relational mapping metadata, DDL scripts, or a combination of the two.
    See [`SourceType`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/SourceType.html) for valid set of values.

    </div>
    <div class="paragraph">

    If no value is specified, a default is assumed as follows:

    </div>
    <div class="ulist">

*   if source scripts are specified (per `javax.persistence.schema-generation.create-script-source`), then `scripts` is assumed
*   otherwise, `metadata` is assumed
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.drop-source`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Specifies whether schema generation commands for schema dropping are to be determine based on object/relational mapping metadata, DDL scripts, or a combination of the two.
    See [`SourceType`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/SourceType.html) for valid set of values.

    </div>
    <div class="paragraph">

    If no value is specified, a default is assumed as follows:

    </div>
    <div class="ulist">

*   if source scripts are specified (per `javax.persistence.schema-generation.create-script-source`), then `scripts` is assumed
*   otherwise, `metadata` is assumed
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.create-script-source`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Specifies the `create` script file as either a `java.io.Reader` configured for reading of the DDL script file or a string designating a file `java.net.URL` for the DDL script.

    Hibernate historically also accepted `hibernate.hbm2ddl.import_files` for a similar purpose, but `javax.persistence.schema-generation.create-script-source` should be preferred over `hibernate.hbm2ddl.import_files`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.drop-script-source`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Specifies the `drop` script file as either a `java.io.Reader` configured for reading of the DDL script file or a string designating a file `java.net.URL` for the DDL script.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.scripts.create-target`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    For cases where the `javax.persistence.schema-generation.scripts.action` value indicates that schema creation commands should be written to DDL script file, `javax.persistence.schema-generation.scripts.create-target` specifies either a `java.io.Writer` configured for output of the DDL script or a string specifying the file URL for the DDL script.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.schema-generation.scripts.drop-target`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    For cases where the `javax.persistence.schema-generation.scripts.action` value indicates that schema dropping commands should be written to DDL script file, `javax.persistence.schema-generation.scripts.drop-target` specifies either a `java.io.Writer` configured for output of the DDL script or a string specifying the file URL for the DDL script.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.hibernate.hbm2ddl.import_files`
    </td>
    <td class="tableblock halign-left valign-top">

    `import.sql` (default value)
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Comma-separated names of the optional files containing SQL DML statements executed during the `SessionFactory` creation.
    File order matters, the statements of a give file are executed before the statements of the following one.

    </div>
    <div class="paragraph">

    These statements are only executed if the schema is created, meaning that `hibernate.hbm2ddl.auto` is set to `create`, `create-drop`, or `update`.
    `javax.persistence.schema-generation.create-script-source` / `javax.persistence.schema-generation.drop-script-source` should be preferred.

    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.sql-load-script-source`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    JPA variant of `hibernate.hbm2ddl.import_files`. Specifies a `java.io.Reader` configured for reading of the SQL load script or a string designating the file `java.net.URL` for the SQL load script.
    A "SQL load script" is a script that performs some database initialization (INSERT, etc).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2ddl.import_files_sql_extractor`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Reference to the [`ImportSqlCommandExtractor`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/hbm2ddl/ImportSqlCommandExtractor.html) implementation class to use for parsing source/import files as defined by `javax.persistence.schema-generation.create-script-source`,
    `javax.persistence.schema-generation.drop-script-source` or `hibernate.hbm2ddl.import_files`.

    Reference may refer to an instance, a Class implementing `ImportSqlCommandExtractor` of the fully-qualified name of the `ImportSqlCommandExtractor` implementation.
    If the fully-qualified name is given, the implementation must provide a no-arg constructor.

    The default value is [`SingleLineSqlCommandExtractor`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/hbm2ddl/SingleLineSqlCommandExtractor.html).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2dll.create_namespaces`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Specifies whether to automatically create also the database schema/catalog.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `javax.persistence.create-database-schemas`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    The JPA variant of `hibernate.hbm2dll.create_namespaces`. Specifies whether the persistence provider is to create the database schema(s) in addition to creating database objects (tables, sequences, constraints, etc).
    The value of this boolean property should be set to `true` if the persistence provider is to create schemas in the database or to generate DDL that contains "CREATE SCHEMA" commands.
    If this property is not supplied (or is explicitly `false`), the provider should not attempt to create database schemas.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2ddl.schema_filter_provider`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Used to specify the [`SchemaFilterProvider`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/spi/SchemaFilterProvider.html) to be used by `create`, `drop`, `migrate`, and `validate` operations on the database schema.
    `SchemaFilterProvider` provides filters that can be used to limit the scope of these operations to specific namespaces, tables and sequences. All objects are included by default.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2ddl.jdbc_metadata_extraction_strategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `grouped` (default value) or `individually`
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Setting to choose the strategy used to access the JDBC Metadata.
    Valid options are defined by the `strategy` value of the [`JdbcMetadaAccessStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/JdbcMetadaAccessStrategy.html) enum:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`grouped`</dt>
    <dd>

    [`SchemaMigrator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/spi/SchemaMigrator.html) and [`SchemaValidator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/spi/SchemaValidator.html) execute a single `java.sql.DatabaseMetaData#getTables(String, String, String, String[])` call to retrieve all the database table in order to determine if all the `javax.persistence.Entity` have a corresponding mapped database tables.

    </dd>
    <dt class="hdlist1">`individually`</dt>
    <dd>

    [`SchemaMigrator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/spi/SchemaMigrator.html) and [`SchemaValidator`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/spi/SchemaValidator.html) execute one `java.sql.DatabaseMetaData#getTables(String, String, String, String[])` call for each `javax.persistence.Entity` in order to determine if a corresponding database table exists.

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2ddl.delimiter`
    </td>
    <td class="tableblock halign-left valign-top">

    `;`
    </td>
    <td class="tableblock halign-left valign-top">

    Identifies the delimiter to use to separate schema management statements in script outputs.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.schema_management_tool`
    </td>
    <td class="tableblock halign-left valign-top">

    A schema name
    </td>
    <td class="tableblock halign-left valign-top">

    Used to specify the [`SchemaManagementTool`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/spi/SchemaManagementTool.html) to use for performing schema management. The default is to use [`HibernateSchemaManagementTool`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/schema/internal/HibernateSchemaManagementTool.html)
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.synonyms`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    If enabled, allows schema update and validation to support synonyms. Due to the possibility that this would return duplicate tables (especially in Oracle), this is disabled by default.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2dll.extra_physical_table_types`
    </td>
    <td class="tableblock halign-left valign-top">

    `BASE TABLE`
    </td>
    <td class="tableblock halign-left valign-top">

    Identifies a comma-separated list of values to specify extra table types, other than the default `TABLE` value, to recognize as defining a physical table by schema update, creation and validation.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.schema_update.unique_constraint_strategy`
    </td>
    <td class="tableblock halign-left valign-top">

    `DROP_RECREATE_QUIETLY`, `RECREATE_QUIETLY`, `SKIP`
    </td>
    <td class="tableblock halign-left valign-top"><div><div class="paragraph">

    Unique columns and unique keys both use unique constraints in most dialects.
    `SchemaUpdate` needs to create these constraints, but DBs support for finding existing constraints is extremely inconsistent.
    Further, non-explicitly-named unique constraints use randomly generated characters.

    </div>
    <div class="paragraph">

    Therefore, the [`UniqueConstraintSchemaUpdateStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/tool/hbm2ddl/UniqueConstraintSchemaUpdateStrategy.html) offers the following options:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`DROP_RECREATE_QUIETLY`</dt>
    <dd>

    Default option.
    Attempt to drop, then (re-)create each unique constraint. Ignore any exceptions being thrown.

    </dd>
    <dt class="hdlist1">`RECREATE_QUIETLY`</dt>
    <dd>

    Attempts to (re-)create unique constraints, ignoring exceptions thrown if the constraint already existed

    </dd>
    <dt class="hdlist1">`SKIP`</dt>
    <dd>

    Does not attempt to create unique constraints on a schema update.

    </dd>
    </dl>
    </div></div></td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2ddl.charset_name`
    </td>
    <td class="tableblock halign-left valign-top">

    `Charset.defaultCharset()`
    </td>
    <td class="tableblock halign-left valign-top">

    Defines the charset (encoding) used for all input/output schema generation resources. By default, Hibernate uses the default charset given by `Charset.defaultCharset()`. This configuration property allows you to override the default JVM setting so that you can specify which encoding is used when reading and writing schema generation resources (e.g. File, URL).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.hbm2ddl.halt_on_error`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Whether the schema migration tool should halt on error, therefore terminating the bootstrap process. By default, the `EntityManagerFactory` or `SessionFactory` are created even if the schema migration throws exceptions. To prevent this default behavior, set this property value to `true`.
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">