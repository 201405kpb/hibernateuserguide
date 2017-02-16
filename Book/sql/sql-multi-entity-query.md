 ### 17.5. Returning multiple entities

    <div class="paragraph">

    Until now, the result set column names are assumed to be the same as the column names specified in the mapping document.
    This can be problematic for SQL queries that join multiple tables since the same column names can appear in more than one table.

    </div>
    <div class="paragraph">

    Column alias injection is needed in the following query which otherwise throws `NonUniqueDiscoveredSqlAliasException`.

    </div>
    <div id="sql-jpa-multi-entity-query-example" class="exampleblock">
    <div class="title">Example 439. JPA native query selecting entities with the same column names</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object&gt; entities = entityManager.createNativeQuery(
        "SELECT * " +
        "FROM person pr, partner pt " +
        "WHERE pr.name = pt.name" )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-multi-entity-query-example" class="exampleblock">
    <div class="title">Example 440. Hibernate native query selecting entities with the same column names</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object&gt; entities = session.createSQLQuery(
            "SELECT * " +
                    "FROM person pr, partner pt " +
                    "WHERE pr.name = pt.name" )
            .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The query was intended to return all `Person` and `Partner` instances with the same name.
    The query fails because there is a conflict of names since the two entities are mapped to the same column names (e.g. `id`, `name`, `version`).
    Also, on some databases the returned column aliases will most likely be on the form `pr.id`, `pr.name`, etc.
    which are not equal to the columns specified in the mappings (`id` and `name`).

    </div>
    <div class="paragraph">

    The following form is not vulnerable to column name duplication:

    </div>
    <div id="sql-hibernate-multi-entity-query-alias-example" class="exampleblock">
    <div class="title">Example 441. Hibernate native query selecting entities with the same column names and aliases</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object&gt; entities = session.createSQLQuery(
        "SELECT {pr.*}, {pt.*} " +
        "FROM person pr, partner pt " +
        "WHERE pr.name = pt.name" )
    .addEntity( "pr", Person.class)
    .addEntity( "pt", Partner.class)
    .list();`</pre>
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

    There&#8217;s no such equivalent in JPA because the `Query` interface doesn&#8217;t define an `addEntity` method equivalent.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `{pr.**}` and `{pt.**}` notation used above is shorthand for "all properties".
    Alternatively, you can list the columns explicitly, but even in this case Hibernate injects the SQL column aliases for each property.
    The placeholder for a column alias is just the property name qualified by the table alias.

    </div>
    </div>
    <div class="sect2">