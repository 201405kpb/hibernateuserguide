#### 12.3.1. HQL/JPQL for UPDATE and DELETE

    <div class="paragraph">

    Both the Hibernate native Query Language and JPQL (Java Persistence Query Language) provide support for bulk UPDATE and DELETE.

    </div>
    <div id="batch-bulk-hql-update-delete-example" class="exampleblock">
    <div class="title">Example 303. Psuedo-syntax for UPDATE and DELETE statements using HQL</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`UPDATE FROM EntityName e WHERE e.name = ?

    DELETE FROM EntityName e WHERE e.name = ?`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The `FROM` and `WHERE` clauses are each optional, but it&#8217;s good practice to use them.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `FROM` clause can only refer to a single entity, which can be aliased.
    If the entity name is aliased, any property references must be qualified using that alias.
    If the entity name is not aliased, then it is illegal for any property references to be qualified.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Joins, either implicit or explicit, are prohibited in a bulk HQL query.
    You can use sub-queries in the `WHERE` clause, and the sub-queries themselves can contain joins.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="batch-bulk-jpql-update-example" class="exampleblock">
    <div class="title">Example 304. Executing a JPQL `UPDATE`, using the `Query.executeUpdate()`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`int updatedEntities = entityManager.createQuery(
        "update Person p " +
        "set p.name = :newName " +
        "where p.name = :oldName" )
    .setParameter( "oldName", oldName )
    .setParameter( "newName", newName )
    .executeUpdate();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="batch-bulk-hql-update-example" class="exampleblock">
    <div class="title">Example 305. Executing an HQL `UPDATE`, using the `Query.executeUpdate()`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`int updatedEntities = session.createQuery(
        "update Person " +
        "set name = :newName " +
        "where name = :oldName" )
    .setParameter( "oldName", oldName )
    .setParameter( "newName", newName )
    .executeUpdate();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In keeping with the EJB3 specification, HQL `UPDATE` statements, by default, do not effect the version or the timestamp property values for the affected entities.
    You can use a versioned update to force Hibernate to reset the version or timestamp property values, by adding the `VERSIONED` keyword after the `UPDATE` keyword.

    </div>
    <div id="batch-bulk-hql-update-version-example" class="exampleblock">
    <div class="title">Example 306. Updating the version of timestamp</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`int updatedEntities = session.createQuery(
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
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If you use the `VERSIONED` statement, you cannot use custom version types, which use class `org.hibernate.usertype.UserVersionType`.

    </div>
    <div class="paragraph">

    This feature is only available in HQL since it&#8217;s not standardized by JPA.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="batch-bulk-jpql-delete-example" class="exampleblock">
    <div class="title">Example 307. A JPQL `DELETE` statement</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`int deletedEntities = entityManager.createQuery(
        "delete Person p " +
        "where p.name = :name" )
    .setParameter( "name", name )
    .executeUpdate();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="batch-bulk-hql-delete-example" class="exampleblock">
    <div class="title">Example 308. An HQL `DELETE` statement</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`int deletedEntities = session.createQuery(
        "delete Person " +
        "where name = :name" )
    .setParameter( "name", name )
    .executeUpdate();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Method `Query.executeUpdate()` returns an `int` value, which indicates the number of entities effected by the operation.
    This may or may not correlate to the number of rows effected in the database.
    An JPQL/HQL bulk operation might result in multiple SQL statements being executed, such as for joined-subclass.
    In the example of joined-subclass, a `DELETE` against one of the subclasses may actually result in deletes in the tables underlying the join, or further down the inheritance hierarchy.

    </div>
    </div>
    <div class="sect3">