 ### 13.8. Caching statistics

    <div class="paragraph">

    If you enable the `hibernate.generate_statistics` configuration property,
    Hibernate will expose a number of metrics via `SessionFactory.getStatistics()`.
    Hibernate can even be configured to expose these statistics via JMX.

    </div>
    <div class="paragraph">

    This way, you can get access to the [`Statistics`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/stat/Statistics.html) class which comprises all sort of
    second-level cache metrics.

    </div>
    <div id="caching-statistics-example" class="exampleblock">
    <div class="title">Example 331. Caching statistics</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Statistics statistics = session.getSessionFactory().getStatistics();
    SecondLevelCacheStatistics secondLevelCacheStatistics =
            statistics.getSecondLevelCacheStatistics( "query.cache.person" );
    long hitCount = secondLevelCacheStatistics.getHitCount();
    long missCount = secondLevelCacheStatistics.getMissCount();
    double hitRatio = (double) hitCount / ( hitCount + missCount );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">
