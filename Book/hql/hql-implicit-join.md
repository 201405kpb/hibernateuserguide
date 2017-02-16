 ### 15.15. Implicit joins (path expressions)

    <div class="paragraph">

    Another means of adding to the scope of object model types available to the query is through the use of implicit joins or path expressions.

    </div>
    <div id="hql-implicit-join-example" class="exampleblock">
    <div class="title">Example 374. Simple implicit join example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = entityManager.createQuery(
        "select ph " +
        "from Phone ph " +
        "where ph.person.address = :address ", Phone.class )
    .setParameter( "address", address )
    .getResultList();

    // same as
    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select ph " +
        "from Phone ph " +
        "join ph.person pr " +
        "where pr.address = :address ", Phone.class )
    .setParameter( "address", address)
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    An implicit join always starts from an `identification variable`, followed by the navigation operator ( `.` ),
    followed by an attribute for the object model type referenced by the initial `identification variable`.
    In the example, the initial `identification variable` is `ph` which refers to the `Phone` entity.
    The `ph.person` reference then refers to the `person` attribute of the `Phone` entity.
    `person` is an association type so we further navigate to its age attribute.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If the attribute represents an entity association (non-collection) or a component/embedded, that reference can be further navigated.
    Basic values and collection-valued associations cannot be further navigated.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    As shown in the example, implicit joins can appear outside the `FROM clause`.
    However, they affect the `FROM clause`.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Implicit joins are always treated as inner joins.

    </div>
    <div class="paragraph">

    Multiple references to the same implicit join always refer to the same logical and physical (SQL) join.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div id="hql-implicit-join-alias-example" class="exampleblock">
    <div class="title">Example 375. Reused implicit join</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = entityManager.createQuery(
        "select ph " +
        "from Phone ph " +
        "where ph.person.address = :address " +
        "  and ph.person.createdOn &gt; :timestamp", Phone.class )
    .setParameter( "address", address )
    .setParameter( "timestamp", timestamp )
    .getResultList();

    //same as
    List&lt;Phone&gt; phones = entityManager.createQuery(
        "select ph " +
        "from Phone ph " +
        "inner join ph.person pr " +
        "where pr.address = :address " +
        "  and pr.createdOn &gt; :timestamp", Phone.class )
    .setParameter( "address", address )
    .setParameter( "timestamp", timestamp )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Just as with explicit joins, implicit joins may reference association or component/embedded attributes.
    For further information about collection-valued association references, see [Collection member references](#hql-collection-valued-associations).

    </div>
    <div class="paragraph">

    In the case of component/embedded attributes, the join is simply logical and does not correlate to a physical (SQL) join.
    Unlike explicit joins, however, implicit joins may also reference basic state fields as long as the path expression ends there.

    </div>
    </div>
    <div class="sect2">