 #### 13.9.2. JCache `CacheManager`

    <div class="paragraph">

    JCache mandates that `CacheManager`s sharing the same URI and class loader be unique in JVM.

    </div>
    <div class="paragraph">

    If you do not specify additional properties, the `JCacheRegionFactory` will load the default JCache provider and create the default `CacheManager`.
    Also, `Cache`s will be created using the default `javax.cache.configuration.MutableConfiguration`.

    </div>
    <div class="paragraph">

    In order to control which provider to use and specify configuration for the `CacheManager` and `Cache`s you can use the following two properties:

    </div>
    <div id="caching-provider-jcache-region-factory-config-example" class="exampleblock">
    <div class="title">Example 333. JCache configuration</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.javax.cache.provider"
        value="org.ehcache.jsr107.EhcacheCachingProvider"/&gt;
    &lt;property
        name="hibernate.javax.cache.uri"
        value="file:/path/to/ehcache.xml"/&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Only by specifying the second property `hibernate.javax.cache.uri` will you be able to have a `CacheManager` per `SessionFactory`.

    </div>
    </div>
    </div>
    <div class="sect2">