 #### 13.7.1. Evicting cache entries

    <div class="paragraph">

    Because the second level cache is bound to the `EntityManagerFactory` or the `SessionFactory`,
    cache eviction must be done through these two interfaces.

    </div>
    <div class="paragraph">

    JPA only supports entity eviction through the [`javax.persistence.Cache`](https://docs.oracle.com/javaee/7/api/javax/persistence/Cache.html) interface:

    </div>
    <div id="caching-management-evict-jpa-example" class="exampleblock">
    <div class="title">Example 329. Evicting entities with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`entityManager.getEntityManagerFactory().getCache().evict( Person.class );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Hibernate is much more flexible in this regard as it offers fine-grained control over what needs to be evicted.
    The [`org.hibernate.Cache`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/Cache.html) interface defines various evicting strategies:

    </div>
    <div class="ulist">

*   entities (by their class or region)
*   entities stored using the natural-id (by their class or region)
*   collections (by the region, and it might take the collection owner identifier as well)
*   queries (by region)
    </div>
    <div id="caching-management-evict-native-example" class="exampleblock">
    <div class="title">Example 330. Evicting entities with Hibernate native API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`session.getSessionFactory().getCache().evictQueryRegion( "query.cache.person" );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
