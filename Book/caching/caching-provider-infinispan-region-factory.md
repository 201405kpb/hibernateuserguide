
    #### 13.11.1. RegionFactory

    <div class="paragraph">

    The hibernate-infinispan module defines two specific providers: `infinispan` and  `infinispan-jndi`.

    </div>
    <div class="sect4">
    
     ##### `InfinispanRegionFactory`

    <div class="paragraph">

    If Hibernate and Infinispan are running in a standalone environment, the `InfinispanRegionFactory` should be configured as follows:

    </div>
    <div id="caching-provider-infinispan-region-factory-basic-example" class="exampleblock">
    <div class="title">Example 337. `InfinispanRegionFactory` configuration</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.region.factory_class"
        value="org.hibernate.cache.infinispan.InfinispanRegionFactory" /&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### `JndiInfinispanRegionFactory`

    <div class="paragraph">

    If the Infinispan `CacheManager` is bound to JNDI, then the `JndiInfinispanRegionFactory` should be used as a region factory:

    </div>
    <div id="caching-provider-infinispan-region-factory-jndi-example" class="exampleblock">
    <div class="title">Example 338. `JndiInfinispanRegionFactory` configuration</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;property
        name="hibernate.cache.region.factory_class"
        value="org.hibernate.cache.infinispan.JndiInfinispanRegionFactory" /&gt;

    &lt;property
        name="hibernate.cache.infinispan.cachemanager"
        value="java:CacheManager" /&gt;`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect4">

    ##### Infinispan in JBoss AS/WildFly

    <div class="paragraph">

    When using JPA in WildFly, region factory is automatically set upon configuring `hibernate.cache.use_second_level_cache=true` (by default second-level cache is not used).
    For more information, please consult [WildFly documentation](https://docs.jboss.org/author/display/WFLY9/JPA+Reference+Guide#JPAReferenceGuide-UsingtheInfinispansecondlevelcache).

    </div>
    </div>
    </div>
    <div class="sect3">