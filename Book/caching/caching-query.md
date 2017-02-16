### 13.6. Query cache

    <div class="paragraph">

    Aside from caching entities and collections, Hibernate offers a query cache too.
    This is useful for frequently executed queries with fixed parameter values.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Caching of query results introduces some overhead in terms of your applications normal transactional processing.
    For example, if you cache results of a query against `Person`,
    Hibernate will need to keep track of when those results should be invalidated because changes have been committed against any `Person` entity.

    </div>
    <div class="paragraph">

    That, coupled with the fact that most applications simply gain no benefit from caching query results,
    leads Hibernate to disable caching of query results by default.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    To use query caching, you will first need to enable it with the following configuration property:

    </div>
    <div id="caching-query-configuration" class="exampleblock">
    <div class="title">Example 318. Enabling query cache</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.use_query_cache"
        value="true" /&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As mentioned above, most queries do not benefit from caching or their results.
    So by default, individual queries are not cached even after enabling query caching.
    Each particular query that needs to be cached must be manually set as cacheable.
    This way, the query looks for existing cache results or adds the query results to the cache when being executed.

    </div>
    <div id="caching-query-jpa-example" class="exampleblock">
    <div class="title">Example 319. Caching query using JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.name = :name", Person.class)
    .setParameter( "name", "John Doe")
    .setHint( "org.hibernate.cacheable", "true")
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-query-native-example" class="exampleblock">
    <div class="title">Example 320. Caching query using Hibernate native API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.name = :name")
    .setParameter( "name", "John Doe")
    .setCacheable(true)
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

    The query cache does not cache the state of the actual entities in the cache;
    it caches only identifier values and results of value type.

    </div>
    <div class="paragraph">

    Just as with collection caching, the query cache should always be used in conjunction with the second-level cache for those entities expected to be cached as part of a query result cache.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect3">