### 17.2. Scalar queries

    <div class="paragraph">

    The most basic SQL query is to get a list of scalars (column) values.

    </div>
    <div id="sql-jpa-all-columns-scalar-query-example" class="exampleblock">
    <div class="title">Example 422. JPA native query selecting all columns</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; persons = entityManager.createNativeQuery(
        "SELECT * FROM person" )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-jpa-custom-column-selection-scalar-query-example" class="exampleblock">
    <div class="title">Example 423. JPA native query with a custom column selection</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; persons = entityManager.createNativeQuery(
        "SELECT id, name FROM person" )
    .getResultList();

    for(Object[] person : persons) {
        Number id = (Number) person[0];
        String name = (String) person[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-all-columns-scalar-query-example" class="exampleblock">
    <div class="title">Example 424. Hibernate native query selecting all columns</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; persons = session.createSQLQuery(
        "SELECT * FROM person" )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="sql-hibernate-custom-column-selection-scalar-query-example" class="exampleblock">
    <div class="title">Example 425. Hibernate native query with a custom column selection</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; persons = session.createSQLQuery(
        "SELECT id, name FROM person" )
    .list();

    for(Object[] person : persons) {
        Number id = (Number) person[0];
        String name = (String) person[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    These will return a `List` of `Object` arrays ( `Object[]` ) with scalar values for each column in the `PERSON` table.
    Hibernate will use `java.sql.ResultSetMetadata` to deduce the actual order and types of the returned scalar values.

    </div>
    <div class="paragraph">

    To avoid the overhead of using `ResultSetMetadata`, or simply to be more explicit in what is returned, one can use `addScalar()`:

    </div>
    <div id="sql-hibernate-scalar-query-explicit-result-set-example" class="exampleblock">
    <div class="title">Example 426. Hibernate native query with explicit result set selection</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; persons = session.createSQLQuery(
        "SELECT * FROM person" )
    .addScalar( "id", LongType.INSTANCE )
    .addScalar( "name", StringType.INSTANCE )
    .list();

    for(Object[] person : persons) {
        Long id = (Long) person[0];
        String name = (String) person[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Although it still returns an `Object` arrays, this query will not use the `ResultSetMetadata` anymore since it explicitly gets the `id` and `name` columns as respectively a `BigInteger` and a `String` from the underlying `ResultSet`.
    This also means that only these two columns will be returned, even though the query is still using `*` and the `ResultSet` contains more than the three listed columns.

    </div>
    <div class="paragraph">

    It is possible to leave out the type information for all or some of the scalars.

    </div>
    <div id="sql-hibernate-scalar-query-partial-explicit-result-set-example" class="exampleblock">
    <div class="title">Example 427. Hibernate native query with result set selection that&#8217;s a partially explicit</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; persons = session.createSQLQuery(
        "SELECT * FROM person" )
    .addScalar( "id", LongType.INSTANCE )
    .addScalar( "name" )
    .list();

    for(Object[] person : persons) {
        Long id = (Long) person[0];
        String name = (String) person[1];
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    This is essentially the same query as before, but now `ResultSetMetaData` is used to determine the type of `name`, where as the type of `id` is explicitly specified.

    </div>
    <div class="paragraph">

    How the `java.sql.Types` returned from `ResultSetMetaData` is mapped to Hibernate types is controlled by the `Dialect`.
    If a specific type is not mapped, or does not result in the expected type, it is possible to customize it via calls to `registerHibernateType` in the Dialect.

    </div>
    </div>
    <div class="sect2">