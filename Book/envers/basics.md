### 21.1. Basics

    <div class="paragraph">

    To audit changes that are performed on an entity, you only need two things:

    </div>
    <div class="ulist">

*   the `hibernate-envers` jar on the classpath,
*   an `@Audited` annotation on the entity.
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Unlike in previous versions, you no longer need to specify listeners in the Hibernate configuration file.
    Just putting the Envers jar on the classpath is enough because listeners will be registered automatically.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    And that&#8217;s all.
    You can create, modify and delete the entities as always.

    </div>
    <div class="paragraph">

    If you look at the generated schema for your entities, or at the data persisted by Hibernate, you will notice that there are no changes.
    However, for each audited entity, a new table is introduced - `entity_table_AUD`, which stores the historical data, whenever you commit a transaction.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Envers automatically creates audit tables if `hibernate.hbm2ddl.auto` option is set to `create`, `create-drop` or `update`.
    Otherwise, to export complete database schema programmatically, use `org.hibernate.envers.tools.hbm2ddl.EnversSchemaGenerator`.
    Appropriate DDL statements can be also generated with Ant task described later in this manual.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Instead of annotating the whole class and auditing all properties, you can annotate only some persistent properties with `@Audited`.
    This will cause only these properties to be audited.

    </div>
    <div class="paragraph">

    The audit (history) of an entity can be accessed using the `AuditReader` interface, which can be obtained having an open `EntityManager` or `Session` via the `AuditReaderFactory`.
    See the [Javadocs]([https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/envers/AuditReaderFactory.html](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/envers/AuditReaderFactory.html)) for these classes for details on the functionality offered.

    </div>
    </div>
    <div class="sect2">