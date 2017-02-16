 #### 15.4.1. Query scrolling

    <div class="paragraph">

    Hibernate offers additional, specialized methods for scrolling the query and handling results using a server-side cursor.

    </div>
    <div class="paragraph">

    `Query#scroll` works in tandem with the JDBC notion of a scrollable `ResultSet`.

    </div>
    <div class="paragraph">

    The `Query#scroll` method is overloaded:

    </div>
    <div class="ulist">

*   Then main form accepts a single argument of type `org.hibernate.ScrollMode` which indicates the type of scrolling to be used.
    See the [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/ScrollMode.html) for the details on each.
*   The second form takes no argument and will use the `ScrollMode` indicated by `Dialect#defaultScrollMode`.
    </div>
    <div class="paragraph">

    `Query#scroll` returns a `org.hibernate.ScrollableResults` which wraps the underlying JDBC (scrollable) `ResultSet` and provides access to the results.
    Unlike a typical forward-only `ResultSet`, the `ScrollableResults` allows you to navigate the `ResultSet` in any direction.

    </div>
    <div id="hql-api-scroll-example" class="exampleblock">
    <div class="title">Example 361. Scrolling through a `ResultSet` containing entities</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`try ( ScrollableResults scrollableResults = session.createQuery(
            "select p " +
            "from Person p " +
            "where p.name like :name" )
            .setParameter( "name", "J%" )
            .scroll()
    ) {
        while(scrollableResults.next()) {
            Person person = (Person) scrollableResults.get()[0];
            process(person);
        }
    }`</pre>
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

    Since this form holds the JDBC `ResultSet` open, the application should indicate when it is done with the `ScrollableResults` by calling its `close()` method (as inherited from `java.io.Closeable`
    so that `ScrollableResults` will work with [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) blocks).

    </div>
    <div class="paragraph">

    If left unclosed by the application, Hibernate will automatically close the underlying resources (e.g. `ResultSet` and `PreparedStatement`) used internally by the `ScrollableResults` when the current transaction is ended (either commit or rollback).

    </div>
    <div class="paragraph">

    However, it is good practice to close the `ScrollableResults` explicitly.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If you plan to use `Query#scroll` with collection fetches it is important that your query explicitly order the results so that the JDBC results contain the related rows sequentially.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Hibernate also supports `Query#iterate`, which is intended for loading entities when it is known that the loaded entries are already stored in the second-level cache.
    The idea behind iterate is that just the matching identifiers will be obtained in the SQL query.
    From these the identifiers are resolved by second-level cache lookup.
    If these second-level cache lookups fail, additional queries will need to be issued against the database.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    This operation can perform significantly better for loading large numbers of entities that for certain already exist in the second-level cache.
    In cases where many of the entities do not exist in the second-level cache, this operation will almost definitely perform worse.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `Iterator` returned from `Query#iterate` is actually a specially typed Iterator: `org.hibernate.engine.HibernateIterator`.
    It is specialized to expose a `close()` method (again, inherited from `java.io.Closeable`).
    When you are done with this `Iterator` you should close it, either by casting to `HibernateIterator` or `Closeable`, or by calling [`Hibernate#close(java.util.Iterator)`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Hibernate.html#close-java.util.Iterator-).

    </div>
    <div class="paragraph">

    Since 5.2, Hibernate offers support for returning a `Stream` which can be later used to transform the underlying `ResultSet`.

    </div>
    <div class="paragraph">

    Internally, the `stream()` behaves like a `Query#scroll` and the underlying result is backed by a `ScrollableResults`.

    </div>
    <div class="paragraph">

    Fetching a projection using the `Query#stream` method can be done as follows:

    </div>
    <div id="hql-api-stream-projection-example" class="exampleblock">
    <div class="title">Example 362. Hibernate `stream()` using a projection result type</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`try ( Stream&lt;Object[]&gt; persons = session.createQuery(
        "select p.name, p.nickName " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" )
    .stream() ) {

        persons
        .map( row -&gt; new PersonNames(
                (String) row[0],
                (String) row[1] ) )
        .forEach( this::process );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When fetching a single result, like a `Person` entity, instead of a `Stream&lt;Object[]&gt;`, Hibernate is going to figure out the actual type, so the result is a `Stream&lt;Person&gt;`.

    </div>
    <div id="hql-api-stream-example" class="exampleblock">
    <div class="title">Example 363. Hibernate `stream()` using an entity result type</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`try( Stream&lt;Person&gt; persons = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" )
    .stream() ) {

        Map&lt;Phone, List&lt;Call&gt;&gt; callRegistry = persons
                .flatMap( person -&gt; person.getPhones().stream() )
                .flatMap( phone -&gt; phone.getCalls().stream() )
                .collect( Collectors.groupingBy( Call::getPhone ) );

        process(callRegistry);
    }`</pre>
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

    Just like with `ScrollableResults`, you should always close a Hibernate `Stream` either explicitly or using a [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) block.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect2">
