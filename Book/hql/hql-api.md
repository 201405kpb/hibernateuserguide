### 15.4. Hibernate Query API

    <div class="paragraph">

    In Hibernate, the HQL query is represented as `org.hibernate.query.Query` which is obtained from a `Session`.
    If the HQL is a named query, `Session#getNamedQuery` would be used; otherwise `Session#createQuery` is needed.

    </div>
    <div id="hql-api-example" class="exampleblock">
    <div class="title">Example 352. Obtaining a Hibernate `Query`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.query.Query query = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name"
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="hql-api-named-query-example" class="exampleblock">
    <div class="title">Example 353. Obtaining a Hibernate `Query` reference for a named query</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.query.Query query = session.getNamedQuery( "get_person_by_name" );`</pre>
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

    Not only was the JPQL syntax heavily inspired by HQL, but many of the JPA APIs were heavily inspired by Hibernate too.
    The two `Query` contracts are very similar.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The Query interface can then be used to control the execution of the query.
    For example, we may want to specify an execution timeout or control caching.

    </div>
    <div id="hql-api-basic-usage-example" class="exampleblock">
    <div class="title">Example 354. Basic Query usage - Hibernate</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.query.Query query = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    // timeout - in seconds
    .setTimeout( 2 )
    // write to L2 caches, but do not read from them
    .setCacheMode( CacheMode.REFRESH )
    // assuming query cache was enabled for the SessionFactory
    .setCacheable( true )
    // add a comment to the generated SQL if enabled via the hibernate.use_sql_comments configuration property
    .setComment( "+ INDEX(p idx_person_name)" );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    For complete details, see the [Query](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Query.html) Javadocs.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Query hints here are database query hints.
    They are added directly to the generated SQL according to `Dialect#getQueryHintString`.
    The JPA notion of query hints, on the other hand, refer to hints that target the provider (Hibernate).
    So even though they are called the same, be aware they have a very different purpose.
    Also, be aware that Hibernate query hints generally make the application non-portable across databases unless the code adding them first checks the Dialect.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Flushing is covered in detail in [Flushing](#flushing).
    Locking is covered in detail in [Locking](#locking).
    The concept of read-only state is covered in [Persistence Contexts](#pc).

    </div>
    <div class="paragraph">

    Hibernate also allows an application to hook into the process of building the query results via the `org.hibernate.transform.ResultTransformer` contract.
    See its [Javadocs](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/transform/ResultTransformer.html) as well as the Hibernate-provided implementations for additional details.

    </div>
    <div class="paragraph">

    The last thing that needs to happen before we can execute the query is to bind the values for any parameters defined in the query.
    Query defines many overloaded methods for this purpose.
    The most generic form takes the value as well as the Hibernate Type.

    </div>
    <div id="hql-api-parameter-example" class="exampleblock">
    <div class="title">Example 355. Hibernate name parameter binding</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.query.Query query = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%", StringType.INSTANCE );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate generally understands the expected type of the parameter given its context in the query.
    In the previous example since we are using the parameter in a `LIKE` comparison against a String-typed attribute Hibernate would automatically infer the type; so the above could be simplified.

    </div>
    <div id="hql-api-parameter-inferred-type-example" class="exampleblock">
    <div class="title">Example 356. Hibernate name parameter binding (inferred type)</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.query.Query query = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    There are also short hand forms for binding common types such as strings, booleans, integers, etc.

    </div>
    <div id="hql-api-parameter-short-form-example" class="exampleblock">
    <div class="title">Example 357. Hibernate name parameter binding (short forms)</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.query.Query query = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name " +
        "  and p.createdOn &gt; :timestamp" )
    .setParameter( "name", "J%" )
    .setParameter( "timestamp", timestamp, TemporalType.TIMESTAMP);`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    HQL-style positional parameters follow JDBC positional parameter syntax.
    They are declared using `?` without a following ordinal.
    There is no way to relate two such positional parameters as being "the same" aside from binding the same value to each.

    </div>
    <div id="hql-api-positional-parameter-example" class="exampleblock">
    <div class="title">Example 358. Hibernate positional parameter binding</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`org.hibernate.query.Query query = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like ? " )
    .setParameter( 0, "J%" );`</pre>
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

    This form should be considered deprecated and may be removed in the near future.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    In terms of execution, Hibernate offers 4 different methods. The 2 most commonly used are

    </div>
    <div class="ulist">

*   `Query#list` - executes the select query and returns back the list of results.
*   `Query#uniqueResult` - executes the select query and returns the single result. If there were more than one result an exception is thrown.
    </div>
    <div id="hql-api-list-example" class="exampleblock">
    <div class="title">Example 359. Hibernate `list()` result</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    It is also possible to extract a single result from a `Query`.

    </div>
    <div id="hql-api-unique-result-example" class="exampleblock">
    <div class="title">Example 360. Hibernate `uniqueResult()`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = (Person) session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" )
    .uniqueResult();`</pre>
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

    If the unique result is used often and the attributes upon which it is based are unique, you may want to consider mapping a natural-id and using the natural-id loading API.
    See the [Natural Ids](#naturalid) for more information on this topic.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect3">
