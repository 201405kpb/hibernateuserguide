 ### 15.8. Update statements

    <div class="paragraph">

    The BNF for `UPDATE` statements is the same in HQL and JPQL:

    </div>
    <div id="hql-update-bnf-example" class="exampleblock">
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`update_statement ::=
        update_clause [where_clause]

    update_clause ::=
        UPDATE entity_name [[AS] identification_variable]
        SET update_item {, update_item}*

    update_item ::=
        [identification_variable.]{state_field | single_valued_object_field} = new_value

    new_value ::=
        scalar_expression | simple_entity_expression | NULL`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    `UPDATE` statements, by default, do not effect the `version` or the `timestamp` attribute values for the affected entities.

    </div>
    <div class="paragraph">

    However, you can force Hibernate to set the `version` or `timestamp` attribute values through the use of a `versioned update`.
    This is achieved by adding the `VERSIONED` keyword after the `UPDATE` keyword.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This is a Hibernate specific feature and will not work in a portable manner.

    </div>
    <div class="paragraph">

    Custom version types, `org.hibernate.usertype.UserVersionType`, are not allowed in conjunction with a `update versioned` statement.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    An `UPDATE` statement is executed using the `executeUpdate()` of either `org.hibernate.query.Query` or `javax.persistence.Query`.
    The method is named for those familiar with the JDBC `executeUpdate()` on `java.sql.PreparedStatement`.

    </div>
    <div class="paragraph">

    The `int` value returned by the `executeUpdate()` method indicates the number of entities effected by the operation.
    This may or may not correlate to the number of rows effected in the database.
    An HQL bulk operation might result in multiple actual SQL statements being executed (for joined-subclass, for example).
    The returned number indicates the number of actual entities affected by the statement.
    Using a JOINED inheritance hierarchy, a delete against one of the subclasses may actually result in deletes against not just the table to which that subclass is mapped, but also the "root" table and tables "in between".

    </div>
    <div id="hql-update-example" class="exampleblock">
    <div class="title">Example 364. UPDATE query statements</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`int updatedEntities = entityManager.createQuery(
        "update Person p " +
        "set p.name = :newName " +
        "where p.name = :oldName" )
    .setParameter( "oldName", oldName )
    .setParameter( "newName", newName )
    .executeUpdate();

    int updatedEntities = session.createQuery(
        "update Person " +
        "set name = :newName " +
        "where name = :oldName" )
    .setParameter( "oldName", oldName )
    .setParameter( "newName", newName )
    .executeUpdate();

    int updatedEntities = session.createQuery(
        "update versioned Person " +
        "set name = :newName " +
        "where name = :oldName" )
    .setParameter( "oldName", oldName )
    .setParameter( "newName", newName )
    .executeUpdate();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Neither `UPDATE` nor `DELETE` statements allow implicit joins. Their form already disallows explicit joins too.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">