 #### 13.1.1. RegionFactory

    <div class="paragraph">

    `org.hibernate.cache.spi.RegionFactory` defines the integration between Hibernate and a pluggable caching provider.
    `hibernate.cache.region.factory_class` is used to declare the provider to use.
    Hibernate comes with built-in support for the Java caching standard [JCache](#caching-provider-jcache)
    and also two popular caching libraries: [Ehcache](#caching-provider-ehcache) and [Infinispan](#caching-provider-infinispan).
    Detailed information is provided later in this chapter.

    </div>
    </div>
    <div class="sect3">