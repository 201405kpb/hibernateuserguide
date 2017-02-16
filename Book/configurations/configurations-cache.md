### 23.10. Cache Properties

    <table class="tableblock frame-all grid-all spread">
    <colgroup>
    <col style="width: 20%;">
    <col style="width: 20%;">
    <col style="width: 60%;">
    </colgroup>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    Property
    </td>
    <td class="tableblock halign-left valign-top">

    Example
    </td>
    <td class="tableblock halign-left valign-top">

    Purpose
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.region.factory_class`
    </td>
    <td class="tableblock halign-left valign-top">

    `org.hibernate.cache.infinispan.
    InfinispanRegionFactory`
    </td>
    <td class="tableblock halign-left valign-top">

    The fully-qualified name of the `RegionFactory` implementation class.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.default_cache_concurrency_strategy`
    </td>
    <td class="tableblock halign-left valign-top"></td>
    <td class="tableblock halign-left valign-top">

    Setting used to give the name of the default [`CacheConcurrencyStrategy`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/CacheConcurrencyStrategy.html) to use when either `@javax.persistence.Cacheable` or
    `@org.hibernate.annotations.Cache`.  `@org.hibernate.annotations.Cache` is used to override the global setting.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.use_minimal_puts`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Optimizes second-level cache operation to minimize writes, at the cost of more frequent reads. This is most useful for clustered caches and is enabled by default for clustered cache implementations.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.use_query_cache`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Enables the query cache. You still need to set individual queries to be cachable.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.use_second_level_cache`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` (default value) or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Enable/disable the second level cache, which is enabled by default, although the default `RegionFactor` is `NoCachingRegionFactory` (meaning there is no actual caching implementation).
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.query_cache_factory`
    </td>
    <td class="tableblock halign-left valign-top">

    Fully-qualified classname
    </td>
    <td class="tableblock halign-left valign-top">

    A custom [`QueryCacheFactory`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/cache/spi/QueryCacheFactory.html) interface. The default is the built-in `StandardQueryCacheFactory`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.region_prefix`
    </td>
    <td class="tableblock halign-left valign-top">

    A string
    </td>
    <td class="tableblock halign-left valign-top">

    A prefix for second-level cache region names.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.use_structured_entries`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default value)
    </td>
    <td class="tableblock halign-left valign-top">

    Forces Hibernate to store data in the second-level cache in a more human-readable format.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.auto_evict_collection_cache`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false` (default: false)
    </td>
    <td class="tableblock halign-left valign-top">

    Enables the automatic eviction of a bi-directional association&#8217;s collection cache when an element in the `ManyToOne` collection is added/updated/removed without properly managing the change on the `OneToMany` side.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.cache.use_reference_entries`
    </td>
    <td class="tableblock halign-left valign-top">

    `true` or `false`
    </td>
    <td class="tableblock halign-left valign-top">

    Optimizes second-level cache operation to store immutable entities (aka "reference") which do not have associations into cache directly, this case, lots of disasseble and deep copy operations can be avoid. Default value of this property is `false`.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.classcache`
    </td>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.classcache
    .org.hibernate.ejb.test.Item` = `read-write`
    </td>
    <td class="tableblock halign-left valign-top">

    Sets the associated entity class cache concurrency strategy for the designated region. Caching configuration should follow the following pattern `hibernate.ejb.classcache.&lt;fully.qualified.Classname&gt;` usage[, region] where usage is the cache strategy used and region the cache region name.
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.collectioncache`
    </td>
    <td class="tableblock halign-left valign-top">

    `hibernate.ejb.collectioncache
    .org.hibernate.ejb.test.Item.distributors` = `read-write, RegionName`/&gt;
    </td>
    <td class="tableblock halign-left valign-top">

    Sets the associated collection cache concurrency strategy for the designated region. Caching configuration should follow the following pattern `hibernate.ejb.collectioncache.&lt;fully.qualified.Classname&gt;.&lt;role&gt;` usage[, region] where usage is the cache strategy used and region the cache region name
    </td>
    </tr>
    </tbody>
    </table>
    </div>
    <div class="sect2">