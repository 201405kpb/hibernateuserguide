 ### 15.3. JPA Query API

    <div class="paragraph">

    In JPA the query is represented by `javax.persistence.Query` or `javax.persistence.TypedQuery` as obtained from the `EntityManager`.
    The create an inline `Query` or `TypedQuery`, you need to use the `EntityManager#createQuery` method.
    For named queries, the `EntityManager#createNamedQuery` method is needed.

    </div>
    <div id="jpql-api-example" class="exampleblock">
    <div class="title">Example 345. Obtaining a JPA `Query` or a `TypedQuery` reference</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Query query = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name"
    );

    TypedQuery&lt;Person&gt; typedQuery = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name", Person.class
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="jpql-api-named-query-example" class="exampleblock">
    <div class="title">Example 346. Obtaining a JPA `Query` or a `TypedQuery` reference for a named query</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@NamedQueries(
        @NamedQuery(
            name = "get_person_by_name",
            query = "select p from Person p where name = :name"
        )
    )

    Query query = entityManager.createNamedQuery( "get_person_by_name" );

    TypedQuery&lt;Person&gt; typedQuery = entityManager.createNamedQuery(
        "get_person_by_name", Person.class
    );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `Query` interface can then be used to control the execution of the query.
    For example, we may want to specify an execution timeout or control caching.

    </div>
    <div id="jpql-api-basic-usage-example" class="exampleblock">
    <div class="title">Example 347. Basic JPA `Query` usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Query query = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    // timeout - in milliseconds
    .setHint( "javax.persistence.query.timeout", 2000 )
    // flush only at commit time
    .setFlushMode( FlushModeType.COMMIT );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    For complete details, see the `Query` [Javadocs](http://docs.oracle.com/javaee/7/api/javax/persistence/Query.html).
    Many of the settings controlling the execution of the query are defined as hints.
    JPA defines some standard hints (like timeout in the example), but most are provider specific.
    Relying on provider specific hints limits your applications portability to some degree.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`javax.persistence.query.timeout`</dt>
    <dd>

    Defines the query timeout, in milliseconds.

    </dd>
    <dt class="hdlist1">`javax.persistence.fetchgraph`</dt>
    <dd>

    Defines a _fetchgraph_ EntityGraph.
    Attributes explicitly specified as `AttributeNodes` are treated as `FetchType.EAGER` (via join fetch or subsequent select).
    For details, see the EntityGraph discussions in [Fetching](#fetching).

    </dd>
    <dt class="hdlist1">`javax.persistence.loadgraph`</dt>
    <dd>

    Defines a _loadgraph_ EntityGraph.
    Attributes explicitly specified as AttributeNodes are treated as `FetchType.EAGER` (via join fetch or subsequent select).
    Attributes that are not specified are treated as `FetchType.LAZY` or `FetchType.EAGER` depending on the attribute&#8217;s definition in metadata.
    For details, see the EntityGraph discussions in [Fetching](#fetching).

    </dd>
    <dt class="hdlist1">`org.hibernate.cacheMode`</dt>
    <dd>

    Defines the `CacheMode` to use. See `org.hibernate.query.Query#setCacheMode`.

    </dd>
    <dt class="hdlist1">`org.hibernate.cacheable`</dt>
    <dd>

    Defines whether the query is cacheable. true/false. See `org.hibernate.query.Query#setCacheable`.

    </dd>
    <dt class="hdlist1">`org.hibernate.cacheRegion`</dt>
    <dd>

    For queries that are cacheable, defines a specific cache region to use. See `org.hibernate.query.Query#setCacheRegion`.

    </dd>
    <dt class="hdlist1">`org.hibernate.comment`</dt>
    <dd>

    Defines the comment to apply to the generated SQL. See `org.hibernate.query.Query#setComment`.

    </dd>
    <dt class="hdlist1">`org.hibernate.fetchSize`</dt>
    <dd>

    Defines the JDBC fetch-size to use. See `org.hibernate.query.Query#setFetchSize`

    </dd>
    <dt class="hdlist1">`org.hibernate.flushMode`</dt>
    <dd>

    Defines the Hibernate-specific `FlushMode` to use. See `org.hibernate.query.Query#setFlushMode.` If possible, prefer using `javax.persistence.Query#setFlushMode` instead.

    </dd>
    <dt class="hdlist1">`org.hibernate.readOnly`</dt>
    <dd>

    Defines that entities and collections loaded by this query should be marked as read-only. See `org.hibernate.query.Query#setReadOnly`

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    The final thing that needs to happen before the query can be executed is to bind the values for any defined parameters.
    JPA defines a simplified set of parameter binding methods.
    Essentially it supports setting the parameter value (by name/position) and a specialized form for `Calendar`/`Date` types additionally accepting a `TemporalType`.

    </div>
    <div id="jpql-api-parameter-example" class="exampleblock">
    <div class="title">Example 348. JPA name parameter binding</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Query query = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" );

    // For generic temporal field types (e.g. `java.util.Date`, `java.util.Calendar`)
    // we also need to provide the associated `TemporalType`
    Query query = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.createdOn &gt; :timestamp" )
    .setParameter( "timestamp", timestamp, TemporalType.DATE );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    JPQL-style positional parameters are declared using a question mark followed by an ordinal - `?1`, `?2`.
    The ordinals start with 1.
    Just like with named parameters, positional parameters can also appear multiple times in a query.

    </div>
    <div id="jpql-api-positional-parameter-example" class="exampleblock">
    <div class="title">Example 349. JPA positional parameter binding</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Query query = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like ?1" )
    .setParameter( 1, "J%" );`</pre>
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

    It&#8217;s good practice not to mix forms in a given query.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    In terms of execution, JPA `Query` offers 2 different methods for retrieving a result set.

    </div>
    <div class="ulist">

*   `Query#getResultList()` - executes the select query and returns back the list of results.
*   `Query#getSingleResult()` - executes the select query and returns a single result. If there were more than one result an exception is thrown.
    </div>
    <div id="hql-api-list-example" class="exampleblock">
    <div class="title">Example 350. JPA `getResultList()` result</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="hql-api-unique-result-example" class="exampleblock">
    <div class="title">Example 351. JPA `getSingleResult()`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = (Person) entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name like :name" )
    .setParameter( "name", "J%" )
    .getSingleResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
