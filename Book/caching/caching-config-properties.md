 #### 13.1.2. Caching configuration properties

    <div class="paragraph">

    Besides specific provider configuration, there are a number of configurations options on the Hibernate side of the integration that control various caching behaviors:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`hibernate.cache.use_second_level_cache`</dt>
    <dd>

    Enable or disable second level caching overall. Default is true, although the default region factory is `NoCachingRegionFactory`.

    </dd>
    <dt class="hdlist1">`hibernate.cache.use_query_cache`</dt>
    <dd>

    Enable or disable second level caching of query results. Default is false.

    </dd>
    <dt class="hdlist1">`hibernate.cache.query_cache_factory`</dt>
    <dd>

    Query result caching is handled by a special contract that deals with staleness-based invalidation of the results.
    The default implementation does not allow stale results at all. Use this for applications that would like to relax that.
    Names an implementation of `org.hibernate.cache.spi.QueryCacheFactory`

    </dd>
    <dt class="hdlist1">`hibernate.cache.use_minimal_puts`</dt>
    <dd>

    Optimizes second-level cache operations to minimize writes, at the cost of more frequent reads. Providers typically set this appropriately.

    </dd>
    <dt class="hdlist1">`hibernate.cache.region_prefix`</dt>
    <dd>

    Defines a name to be used as a prefix to all second-level cache region names.

    </dd>
    <dt class="hdlist1">`hibernate.cache.default_cache_concurrency_strategy`</dt>
    <dd>

    In Hibernate second-level caching, all regions can be configured differently including the concurrency strategy to use when accessing that particular region.
    This setting allows to define a default strategy to be used.
    This setting is very rarely required as the pluggable providers do specify the default strategy to use.
    Valid values include:

    <div class="ulist">

*   read-only,
*   read-write,
*   nonstrict-read-write,
*   transactional
    </div>
    </dd>
    <dt class="hdlist1">`hibernate.cache.use_structured_entries`</dt>
    <dd>

    If `true`, forces Hibernate to store data in the second-level cache in a more human-friendly format.
    Can be useful if you&#8217;d like to be able to "browse" the data directly in your cache, but does have a performance impact.

    </dd>
    <dt class="hdlist1">`hibernate.cache.auto_evict_collection_cache`</dt>
    <dd>

    Enables or disables the automatic eviction of a bidirectional association&#8217;s collection cache entry when the association is changed just from the owning side.
    This is disabled by default, as it has a performance impact to track this state.
    However if your application does not manage both sides of bidirectional association where the collection side is cached,
    the alternative is to have stale data in that collection cache.

    </dd>
    <dt class="hdlist1">`hibernate.cache.use_reference_entries`</dt>
    <dd>

    Enable direct storage of entity references into the second level cache for read-only or immutable entities.

    </dd>
    <dt class="hdlist1">`hibernate.cache.keys_factory`</dt>
    <dd>

    When storing entries into second-level cache as key-value pair, the identifiers can be wrapped into tuples
    &lt;entity type, tenant, identifier&gt; to guarantee uniqueness in case that second-level cache stores all entities
    in single space. These tuples are then used as keys in the cache. When the second-level cache implementation
    (incl. its configuration) guarantees that different entity types are stored separately and multi-tenancy is not
    used, you can omit this wrapping to achieve better performance. Currently, this property is only supported when
    Infinispan is configured as the second-level cache implementation. Valid values are:

    <div class="ulist">

*   `default` (wraps identitifers in the tuple)
*   `simple` (uses identifiers as keys without any wrapping)
*   fully qualified class name that implements `org.hibernate.cache.spi.CacheKeysFactory`
    </div>
    </dd>
    </dl>
    </div>
    </div>
    </div>
    <div class="sect2">