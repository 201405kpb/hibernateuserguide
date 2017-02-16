 ### 17.4. Handling associations and collections

    <div class="paragraph">

    If the entity is mapped with a `many-to-one` or a child-side `one-to-one` to another entity, it is required to also return this when performing the native query,
    otherwise a database specific _column not found_ error will occur.

    </div>
    <div id="sql-jpa-entity-associations-query-many-to-one-example" class="exampleblock">
    <div class="title">Example 432. JPA native query selecting entities with many-to-one association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = entityManager.createNativeQuery(
        "SELECT id, phone_number, phone_type, person_id " +
        "FROM phone", Phone.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-entity-associations-query-many-to-one-example" class="exampleblock">
    <div class="title">Example 433. Hibernate native query selecting entities with many-to-one association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = session.createSQLQuery(
        "SELECT id, phone_number, phone_type, person_id " +
        "FROM phone" )
    .addEntity( Phone.class )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This will allow the `Phone#person` to function properly.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The additional columns will automatically be returned when using the `*` notation.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    It is possible to eagerly join the `Phone` and the `Person` entities to avoid the possible extra roundtrip for initializing the `many-to-one` association.

    </div>
    <div id="sql-jpa-entity-associations-query-many-to-one-join-example" class="exampleblock">
    <div class="title">Example 434. JPA native query selecting entities with joined many-to-one association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = entityManager.createNativeQuery(
        "SELECT * " +
        "FROM phone ph " +
        "JOIN person pr ON ph.person_id = pr.id", Phone.class )
    .getResultList();

    for(Phone phone : phones) {
        Person person = phone.getPerson();
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT id ,
           number ,
           type ,
           person_id
    FROM   phone`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-entity-associations-query-many-to-one-join-example" class="exampleblock">
    <div class="title">Example 435. Hibernate native query selecting entities with joined many-to-one association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = session.createSQLQuery(
        "SELECT * " +
        "FROM phone ph " +
        "JOIN person pr ON ph.person_id = pr.id" )
    .addEntity("phone", Phone.class )
    .addJoin( "pr", "phone.person")
    .list();

    for(Object[] tuple : tuples) {
        Phone phone = (Phone) tuple[0];
        Person person = (Person) tuple[1];
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT id ,
           number ,
           type ,
           person_id
    FROM   phone`</pre>
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

    As seen in the associated SQL query, Hibernate manages to construct the entity hierarchy without requiring any extra database roundtrip.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    By default, when using the `addJoin()` method, the result set will contain both entities that are joined.
    To construct the entity hierarchy, you need to use a `ROOT_ENTITY` or `DISTINCT_ROOT_ENTITY` `ResultTransformer`.

    </div>
    <div id="sql-hibernate-entity-associations-query-many-to-one-join-result-transformer-example" class="exampleblock">
    <div class="title">Example 436. Hibernate native query selecting entities with joined many-to-one association and `ResultTransformer`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createSQLQuery(
        "SELECT * " +
        "FROM phone ph " +
        "JOIN person pr ON ph.person_id = pr.id" )
    .addEntity("phone", Phone.class )
    .addJoin( "pr", "phone.person")
    .setResultTransformer( Criteria.ROOT_ENTITY )
    .list();

    for(Person person : persons) {
        person.getPhones();
    }`</pre>
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

    Because of the `ROOT_ENTITY` `ResultTransformer`, this query will return the parent-side as root entities.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Notice that you added an alias name _pr_ to be able to specify the target property path of the join.
    It is possible to do the same eager joining for collections (e.g. the `Phone#calls` `one-to-many` association).

    </div>
    <div id="sql-jpa-entity-associations-query-one-to-many-join-example" class="exampleblock">
    <div class="title">Example 437. JPA native query selecting entities with joined one-to-many association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = entityManager.createNativeQuery(
        "SELECT * " +
        "FROM phone ph " +
        "JOIN phone_call c ON c.phone_id = ph.id", Phone.class )
    .getResultList();

    for(Phone phone : phones) {
        List&lt;Call&gt; calls = phone.getCalls();
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT *
    FROM phone ph
    JOIN call c ON c.phone_id = ph.id`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-entity-associations-query-one-to-many-join-example" class="exampleblock">
    <div class="title">Example 438. Hibernate native query selecting entities with joined one-to-many association</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; tuples = session.createSQLQuery(
        "SELECT * " +
        "FROM phone ph " +
        "JOIN phone_call c ON c.phone_id = ph.id" )
    .addEntity("phone", Phone.class )
    .addJoin( "c", "phone.calls")
    .list();

    for(Object[] tuple : tuples) {
        Phone phone = (Phone) tuple[0];
        Call call = (Call) tuple[1];
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT *
    FROM phone ph
    JOIN call c ON c.phone_id = ph.id`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    At this stage you are reaching the limits of what is possible with native queries, without starting to enhance the sql queries to make them usable in Hibernate.
    Problems can arise when returning multiple entities of the same type or when the default alias/column names are not enough.

    </div>
    </div>
    <div class="sect2">