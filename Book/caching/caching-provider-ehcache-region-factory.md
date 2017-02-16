
    #### 13.10.1. RegionFactory

    <div class="paragraph">

    The hibernate-ehcache module defines two specific region factories: `EhCacheRegionFactory` and `SingletonEhCacheRegionFactory`.

    </div>
    <div class="sect4">

    ##### `EhCacheRegionFactory`

    <div class="paragraph">

    To use the `EhCacheRegionFactory`, you need to specify the following configuration property:

    </div>
    <div id="caching-provider-ehcache-region-factory-shared-example" class="exampleblock">
    <div class="title">Example 334. `EhCacheRegionFactory` configuration</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.region.factory_class"
        value="org.hibernate.cache.ehcache.EhCacheRegionFactory"/&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `EhCacheRegionFactory` configures a `net.sf.ehcache.CacheManager` for each `SessionFactory`,
    so the `CacheManager` is not shared among multiple `SessionFactory` instances in the same JVM.

    </div>
    </div>
    <div class="sect4">

    ##### `SingletonEhCacheRegionFactory`

    <div class="paragraph">

    To use the `SingletonEhCacheRegionFactory`, you need to specify the following configuration property:

    </div>
    <div id="caching-provider-ehcache-region-factory-singleton-example" class="exampleblock">
    <div class="title">Example 335. `SingletonEhCacheRegionFactory` configuration</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.region.factory_class"
        value="org.hibernate.cache.ehcache.SingletonEhCacheRegionFactory"/&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `SingletonEhCacheRegionFactory` configures a singleton `net.sf.ehcache.CacheManager` (see [CacheManager#create()](http://www.ehcache.org/apidocs/2.8.4/net/sf/ehcache/CacheManager.html#create%28%29)),
    shared among multiple `SessionFactory` instances in the same JVM.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    [Ehcache documentation](http://www.ehcache.org/documentation/2.8/integrations/hibernate#optional) recommends using multiple non-singleton `CacheManager(s)` when there are multiple Hibernate `SessionFactory` instances running in the same JVM.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
