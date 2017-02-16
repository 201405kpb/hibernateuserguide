 #### 13.6.1. Query cache regions

    <div class="paragraph">

    This setting creates two new cache regions:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`org.hibernate.cache.internal.StandardQueryCache`</dt>
    <dd>

    Holding the cached query results

    </dd>
    <dt class="hdlist1">`org.hibernate.cache.spi.UpdateTimestampsCache`</dt>
    <dd>

    Holding timestamps of the most recent updates to queryable tables.
    These are used to validate the results as they are served from the query cache.

    </dd>
    </dl>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If you configure your underlying cache implementation to use expiration, it&#8217;s very important that the timeout of the underlying cache region for the `UpdateTimestampsCache` is set to a higher value than the timeouts of any of the query caches.

    </div>
    <div class="paragraph">

    In fact, we recommend that the `UpdateTimestampsCache` region is not configured for expiration (time-based) or eviction (size/memory-based) at all.
    Note that an LRU (Least Recently Used) cache eviction policy is never appropriate for this particular cache region.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    If you require fine-grained control over query cache expiration policies,
    you can specify a named cache region for a particular query.

    </div>
    <div id="caching-query-region-jpa-example" class="exampleblock">
    <div class="title">Example 321. Caching query in custom region using JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
            "select p " +
            "from Person p " +
            "where p.id &gt; :id", Person.class)
            .setParameter( "id", 0L)
            .setHint( QueryHints.HINT_CACHEABLE, "true")
            .setHint( QueryHints.HINT_CACHE_REGION, "query.cache.person" )
            .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-query-region-native-example" class="exampleblock">
    <div class="title">Example 322. Caching query in custom region using Hibernate native API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.id &gt; :id")
    .setParameter( "id", 0L)
    .setCacheable(true)
    .setCacheRegion( "query.cache.person" )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    If you want to force the query cache to refresh one of its regions (disregarding any cached results it finds there),
    you can use custom cache modes.

    </div>
    <div id="caching-query-region-store-mode-jpa-example" class="exampleblock">
    <div class="title">Example 323. Using custom query cache mode with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.id &gt; :id", Person.class)
    .setParameter( "id", 0L)
    .setHint( QueryHints.HINT_CACHEABLE, "true")
    .setHint( QueryHints.HINT_CACHE_REGION, "query.cache.person" )
    .setHint( "javax.persistence.cache.storeMode", CacheStoreMode.REFRESH )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-query-region-native-example" class="exampleblock">
    <div class="title">Example 324. Using custom query cache mode with Hibernate native API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createQuery(
        "select p " +
        "from Person p " +
        "where p.id &gt; :id")
    .setParameter( "id", 0L)
    .setCacheable(true)
    .setCacheRegion( "query.cache.person" )
    .setCacheMode( CacheMode.REFRESH )
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

    When using [`CacheStoreMode.REFRESH`](http://docs.oracle.com/javaee/7/api/javax/persistence/CacheStoreMode.html#REFRESH) or [`CacheMode.REFRESH`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/CacheMode.html#REFRESH) in conjunction with the region you have defined for the given query,
    Hibernate will selectively force the results cached in that particular region to be refreshed.

    </div>
    <div class="paragraph">

    This is particularly useful in cases where underlying data may have been updated via a separate process
    and is a far more efficient alternative to bulk eviction of the region via `SessionFactory` eviction which looks as follows:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`session.getSessionFactory().getCache().evictQueryRegion( "query.cache.person" );`</pre>
    </div>
    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    <div class="sect2">