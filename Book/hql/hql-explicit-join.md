### 15.14. Explicit joins

    <div class="paragraph">

    The `FROM` clause can also contain explicit relationship joins using the `join` keyword.
    These joins can be either `inner` or `left outer` style joins.

    </div>
    <div id="hql-explicit-inner-join-example" class="exampleblock">
    <div class="title">Example 369. Explicit inner join examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select distinct pr " +
        "from Person pr " +
        "join pr.phones ph " +
        "where ph.type = :phoneType", Person.class )
    .setParameter( "phoneType", PhoneType.MOBILE )
    .getResultList();

    // same query but specifying join type as 'inner' explicitly
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select distinct pr " +
        "from Person pr " +
        "inner join pr.phones ph " +
        "where ph.type = :phoneType", Person.class )
    .setParameter( "phoneType", PhoneType.MOBILE )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="hql-explicit-outer-join-example" class="exampleblock">
    <div class="title">Example 370. Explicit left (outer) join examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select distinct pr " +
        "from Person pr " +
        "left join pr.phones ph " +
        "where ph is null " +
        "   or ph.type = :phoneType", Person.class )
    .setParameter( "phoneType", PhoneType.LAND_LINE )
    .getResultList();

    // functionally the same query but using the 'left outer' phrase
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select distinct pr " +
        "from Person pr " +
        "left outer join pr.phones ph " +
        "where ph is null " +
        "   or ph.type = :phoneType", Person.class )
    .setParameter( "phoneType", PhoneType.LAND_LINE )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    HQL also defines a `WITH` clause to qualify the join conditions.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This is specific to HQL. JPQL defines the `ON` clause for this feature.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="hql-explicit-join-with-example" class="exampleblock">
    <div class="title">Example 371. HQL `WITH` clause join example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; personsAndPhones = session.createQuery(
        "select pr.name, ph.number " +
        "from Person pr " +
        "left join pr.phones ph with ph.type = :phoneType " )
    .setParameter( "phoneType", PhoneType.LAND_LINE )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="hql-explicit-join-jpql-on-example" class="exampleblock">
    <div class="title">Example 372. JPQL `ON` clause join example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; personsAndPhones = entityManager.createQuery(
        "select pr.name, ph.number " +
        "from Person pr " +
        "left join pr.phones ph on ph.type = :phoneType " )
    .setParameter( "phoneType", PhoneType.LAND_LINE )
    .getResultList();`</pre>
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

    The important distinction is that in the generated SQL the conditions of the `WITH/ON` clause are made part of the `ON` clause in the generated SQL,
    as opposed to the other queries in this section where the HQL/JPQL conditions are made part of the `WHERE` clause in the generated SQL.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The distinction in this specific example is probably not that significant.
    The `with clause` is sometimes necessary for more complicated queries.

    </div>
    <div class="paragraph">

    Explicit joins may reference association or component/embedded attributes.
    In the case of component/embedded attributes, the join is simply logical and does not correlate to a physical (SQL) join.
    For further information about collection-valued association references, see [Collection member references](#hql-collection-valued-associations).

    </div>
    <div class="paragraph">

    An important use case for explicit joins is to define `FETCH JOINS` which override the laziness of the joined association.
    As an example, given an entity named `Person` with a collection-valued association named `phones`, the `JOIN FETCH` will also load the child collection in the same SQL query:

    </div>
    <div id="hql-explicit-fetch-join-example" class="exampleblock">
    <div class="title">Example 373. Fetch join example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// functionally the same query but using the 'left outer' phrase
    List&lt;Person&gt; persons = entityManager.createQuery(
        "select distinct pr " +
        "from Person pr " +
        "left join fetch pr.phones ", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see from the example, a fetch join is specified by injecting the keyword `fetch` after the keyword `join`.
    In the example, we used a left outer join because we also wanted to return customers who have no orders.

    </div>
    <div class="paragraph">

    Inner joins can also be fetched, but inner joins filter out the root entity.
    In the example, using an inner join instead would have resulted in customers without any orders being filtered out of the result.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Fetch joins are not valid in sub-queries.

    </div>
    <div class="paragraph">

    Care should be taken when fetch joining a collection-valued association which is in any way further restricted (the fetched collection will be restricted too).
    For this reason, it is usually considered best practice not to assign an identification variable to fetched joins except for the purpose of specifying nested fetch joins.

    </div>
    <div class="paragraph">

    Fetch joins should not be used in paged queries (e.g. `setFirstResult()` or `setMaxResults()`), nor should they be used with the `scroll()` or `iterate()` features.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">