 #### 13.9.1. RegionFactory

    <div class="paragraph">

    The `hibernate-jcache` module defines the following region factory: `JCacheRegionFactory`.

    </div>
    <div class="paragraph">

    To use the `JCacheRegionFactory`, you need to specify the following configuration property:

    </div>
    <div id="caching-provider-jcache-region-factory-example" class="exampleblock">
    <div class="title">Example 332. `JCacheRegionFactory` configuration</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.region.factory_class"
        value="org.hibernate.cache.jcache.JCacheRegionFactory"/&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `JCacheRegionFactory` configures a `javax.cache.CacheManager`.

    </div>
    </div>
    <div class="sect3">
