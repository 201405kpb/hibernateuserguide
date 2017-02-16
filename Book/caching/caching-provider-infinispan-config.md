 #### 13.11.2. Configuration properties

    <div class="paragraph">

    Hibernate-infinispan module comes with default configuration in `infinispan-config.xml` that is suited for clustered use. If there&#8217;s only single instance accessing the DB, you can use more performant `infinispan-config-local.xml` by setting the `hibernate.cache.infinispan.cfg` property. If you require further tuning of the cache, you can provide your own configuration. Caches that are not specified in the provided configuration will default to `infinispan-config.xml` (if the provided configuration uses clustering) or `infinispan-config-local.xml`. It is not possible to specify the configuration this way in WildFly.

    </div>
    <div id="caching-provider-infinispan-config-example" class="exampleblock">
    <div class="title">Example 339. Use custom Infinispan configuration</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.infinispan.cfg"
        value="my-infinispan-configuration.xml" /&gt;`</pre>
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

    If the cache is configured as transactional, InfinispanRegionFactory automatically sets transaction manager so that the TM used by Infinispan is the same as TM used by Hibernate.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Cache configuration can differ for each type of data stored in the cache. In order to override the cache configuration template, use property `hibernate.cache.infinispan._data-type_.cfg` where `_data-type_` can be one of:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`entity`</dt>
    <dd>

    Entities indexed by `@Id` or `@EmbeddedId` attribute.

    </dd>
    <dt class="hdlist1">`immutable-entity`</dt>
    <dd>

    Entities tagged with `@Immutable` annotation or set as `mutable=false` in mapping file.

    </dd>
    <dt class="hdlist1">`naturalid`</dt>
    <dd>

    Entities indexed by their `@NaturalId` attribute.

    </dd>
    <dt class="hdlist1">`collection`</dt>
    <dd>

    All collections.

    </dd>
    <dt class="hdlist1">`timestamps`</dt>
    <dd>

    Mapping _entity type_ &#8594; _last modification timestamp_. Used for query caching.

    </dd>
    <dt class="hdlist1">`query`</dt>
    <dd>

    Mapping _query_ &#8594; _query result_.

    </dd>
    <dt class="hdlist1">`pending-puts`</dt>
    <dd>

    Auxiliary caches for regions using invalidation mode caches.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    For specifying cache template for specific region, use region name instead of the `_data-type_`:

    </div>
    <div id="caching-provider-infinispan-config-cache-example" class="exampleblock">
    <div class="title">Example 340. Use custom cache template</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.infinispan.entities.cfg"
        value="custom-entities" /&gt;
    &lt;property
        name="hibernate.cache.infinispan.query.cfg"
        value="custom-query-cache" /&gt;
    &lt;property
        name="hibernate.cache.infinispan.com.example.MyEntity.cfg"
        value="my-entities" /&gt;
    &lt;property
        name="hibernate.cache.infinispan.com.example.MyEntity.someCollection.cfg"
        value="my-entities-some-collection" /&gt;`</pre>
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

    Cache configurations are used only as a template for the cache created for given region (usually each entity hierarchy or collection has its own region). It is not possible to use the same cache for different regions.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Some options in the cache configuration can also be overridden directly through properties. These are:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`hibernate.cache.infinispan._something_.eviction.strategy`</dt>
    <dd>

    Available options are `NONE`, `LRU` and `LIRS`.

    </dd>
    <dt class="hdlist1">`hibernate.cache.infinispan._something_.eviction.max_entries`</dt>
    <dd>

    Maximum number of entries in the cache.

    </dd>
    <dt class="hdlist1">`hibernate.cache.infinispan._something_.expiration.lifespan`</dt>
    <dd>

    Lifespan of entry from insert into cache (in milliseconds)

    </dd>
    <dt class="hdlist1">`hibernate.cache.infinispan._something_.expiration.max_idle`</dt>
    <dd>

    Lifespan of entry from last read/modification (in milliseconds)

    </dd>
    <dt class="hdlist1">`hibernate.cache.infinispan._something_.expiration.wake_up_interval`</dt>
    <dd>

    Period of thread checking expired entries.

    </dd>
    <dt class="hdlist1">`hibernate.cache.infinispan.statistics`</dt>
    <dd>

    Globally enables/disable Infinispan statistics collection, and their exposure via JMX.

    </dd>
    </dl>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    In versions prior to 5.1, `hibernate.cache.infinispan._something_.expiration.wake_up_interval` was called `hibernate.cache.infinispan._something_.eviction.wake_up_interval`.
    Eviction settings are checked upon each cache insert, it is expiration that needs to be triggered periodically.
    The old property still works, but its use is deprecated.

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

    Property `hibernate.cache.infinispan.use_synchronization` that allowed to register Infinispan as XA resource in the transaction has been deprecated in 5.0 and is not honored anymore. Infinispan 2LC must register as synchronizations on transactional caches. Also, non-transactional cache modes hook into the current JTA/JDBC transaction as synchronizations.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect4">

    ##### Configuring Query and Timestamps caches

    <div class="paragraph">

    Since version 5.0 it is possible to configure query caches as _non-transactional_. Consistency guarantees are not changed and writes to the query cache should be faster.

    </div>
    <div class="paragraph">

    The query cache is configured so that queries are only cached locally . Alternatively, you can configure query caching to use replication by selecting the "replicated-query" as query cache name. However, replication for query cache only makes sense if, and only if, all of this conditions are true:

    </div>
    <div class="ulist">

*   Performing the query is quite expensive.
*   The same query is very likely to be repeatedly executed on different cluster nodes.
*   The query is unlikely to be invalidated out of the cache
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Hibernate must aggressively invalidate query results from the cache any time any instance of one of the entity types is modified. All cached query results referencing given entity type are invalidated, even if the change made to the specific entity instance would not have affected the query result.
    The timestamps cache plays here an important role - it contains last modification timestamp for each entity type. After a cached query results is loaded, its timestamp is compared to all timestamps of the entity types that are referenced in the query and if any of these is higher, the cached query result is discarded and the query is executed against DB.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    In default configuration, timestamps cache is asynchronously replicated. This means that a cached query on remote node can provide stale results for a brief time window before the remote timestamps cache is updated. However, requiring synchronous RPC would result in severe performance degradation.

    </div>
    <div class="paragraph">

    Further, but possibly outdated information can be found in [Infinispan documentation](http://infinispan.org/docs/8.0.x/user_guide/user_guide.html#_using_infinispan_as_jpa_hibernate_second_level_cache_provider).

    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">