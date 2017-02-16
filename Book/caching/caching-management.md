### 13.7. Managing the cached data

    <div class="paragraph">

    Traditionally, Hibernate defined the [`CacheMode`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/CacheMode.html) enumeration to describe
    the ways of interactions with the cached data.
    JPA split cache modes by storage ([`CacheStoreMode`](http://docs.oracle.com/javaee/7/api/javax/persistence/CacheStoreMode.html))
    and retrieval ([`CacheRetrieveMode`](http://docs.oracle.com/javaee/7/api/javax/persistence/CacheRetrieveMode.html)).

    </div>
    <div class="paragraph">

    The relationship between Hibernate and JPA cache modes can be seen in the following table:

    </div>
    <table class="tableblock frame-all grid-all spread">
    <caption class="title">Table 5. Cache modes relationships</caption>
    <colgroup>
    <col style="width: %;">
    <col style="width: %;">
    <col style="width: %;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Hibernate</th>
    <th class="tableblock halign-left valign-top">JPA</th>
    <th class="tableblock halign-left valign-top">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    `CacheMode.NORMAL`
    </td>
    <td class="tableblock halign-left valign-top">

    `CacheStoreMode.USE` and `CacheRetrieveMode.USE`
    </td>
    <td class="tableblock halign-left valign-top">

    Default. Reads/writes data from/into cache
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `CacheMode.REFRESH`
    </td>
    <td class="tableblock halign-left valign-top">

    `CacheStoreMode.REFRESH` and `CacheRetrieveMode.BYPASS`
    </td>
    <td class="tableblock halign-left valign-top">

    Doesn&#8217;t read from cache, but writes to the cache upon loading from the database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `CacheMode.PUT`
    </td>
    <td class="tableblock halign-left valign-top">

    `CacheStoreMode.USE` and `CacheRetrieveMode.BYPASS`
    </td>
    <td class="tableblock halign-left valign-top">

    Doesn&#8217;t read from cache, but writes to the cache as it reads from the database
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `CacheMode.GET`
    </td>
    <td class="tableblock halign-left valign-top">

    `CacheStoreMode.BYPASS` and `CacheRetrieveMode.USE`
    </td>
    <td class="tableblock halign-left valign-top">

    Read from the cache, but doesn&#8217;t write to cache
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `CacheMode.IGNORE`
    </td>
    <td class="tableblock halign-left valign-top">

    `CacheStoreMode.BYPASS` and `CacheRetrieveMode.BYPASS`
    </td>
    <td class="tableblock halign-left valign-top">

    Doesn&#8217;t read/write data from/into cache
    </td>
    </tr>
    </tbody>
    </table>
    <div class="paragraph">

    Setting the cache mode can be done either when loading entities directly or when executing a query.

    </div>
    <div id="caching-management-cache-mode-entity-jpa-example" class="exampleblock">
    <div class="title">Example 325. Using custom cache modes with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Map&lt;String, Object&gt; hints = new HashMap&lt;&gt;(  );
    hints.put( "javax.persistence.cache.retrieveMode " , CacheRetrieveMode.USE );
    hints.put( "javax.persistence.cache.storeMode" , CacheStoreMode.REFRESH );
    Person person = entityManager.find( Person.class, 1L , hints);`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-management-cache-mode-entity-native-example" class="exampleblock">
    <div class="title">Example 326. Using custom cache modes with Hibernate native API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`session.setCacheMode( CacheMode.REFRESH );
    Person person = session.get( Person.class, 1L );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The custom cache modes can be set for queries as well:

    </div>
    <div id="caching-management-cache-mode-query-jpa-example" class="exampleblock">
    <div class="title">Example 327. Using custom cache modes for queries with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p from Person p", Person.class)
    .setHint( QueryHints.HINT_CACHEABLE, "true")
    .setHint( "javax.persistence.cache.retrieveMode " , CacheRetrieveMode.USE )
    .setHint( "javax.persistence.cache.storeMode" , CacheStoreMode.REFRESH )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-management-cache-mode-query-native-example" class="exampleblock">
    <div class="title">Example 328. Using custom cache modes for queries with Hibernate native API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = session.createQuery(
        "select p from Person p" )
    .setCacheable( true )
    .setCacheMode( CacheMode.REFRESH )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">