 ### 25.7. Caching

    <div class="paragraph">

    Hibernate has two caching layers:

    </div>
    <div class="ulist">

*   the first-level cache (Persistence Context) which is a application-level repeatable reads.
*   the second-level cache which, unlike application-level caches, it doesn&#8217;t store entity aggregates but normalized dehydrated entity entries.
    </div>
    <div class="paragraph">

    The first-level cache is not a caching solution "per se", being more useful for ensuring `READ COMMITTED` isolation level.

    </div>
    <div class="paragraph">

    While the first-level cache is short lived, being cleared when the underlying `EntityManager` is closed, the second-level cache is tied to an `EntityManagerFactory`.
    Some second-level caching providers offer support for clusters. Therefore, a node needs only to store a subset of the whole cached data.

    </div>
    <div class="paragraph">

    Although the second-level cache can reduce transaction response time since entities are retrieved from the cache rather than from the database,
    there are other options to achieve the same goal,
    and you should consider these alternatives prior to jumping to a second-level cache layer:

    </div>
    <div class="ulist">

*   tuning the underlying database cache so that the working set fits into memory, therefore reducing Disk I/O traffic.
*   optimizing database statements through JDBC batching, statement caching, indexing can reduce the average response time, therefore increasing throughput as well.
*   database replication is also a very valuable option to increase read-only transaction throughput
    </div>
    <div class="paragraph">

    After properly tuning the database, to further reduce the average response time and increase the system throughput, application-level caching becomes inevitable.

    </div>
    <div class="paragraph">

    Topically, a key-value application-level cache like [Memcached](https://memcached.org/) or [Redis](http://redis.io/) is a common choice to store data aggregates.
    If you can duplicate all data in the key-value store, you have the option of taking down the database system for maintenance without completely loosing availability since read-only traffic can still be served from the cache.

    </div>
    <div class="paragraph">

    One of the main challenges of using an application-level cache is ensuring data consistency across entity aggregates.
    That&#8217;s where the second-level cache comes to the rescue.
    Being tightly integrated with Hibernate, the second-level cache can provide better data consistency since entries are cached in a normalized fashion, just like in a relational database.
    Changing a parent entity only requires a single entry cache update, as opposed to cache entry invalidation cascading in key-value stores.

    </div>
    <div class="paragraph">

    The second-level cache provides four cache concurrency strategies:

    </div>
    <div class="ulist">

*   `READ_ONLY`
*   `NONSTRICT_READ_WRITE`
*   `READ_WRITE`
*   `TRANSACTIONAL`
    </div>
    <div class="paragraph">

    `READ_WRITE` is a very good default concurrency strategy since it provides strong consistency guarantees without compromising throughput.
    The `TRANSACTIONAL` concurrency strategy uses JTA. Hence, it&#8217;s more suitable when entities are frequently modified.

    </div>
    <div class="paragraph">

    Both `READ_WRITE` and `TRANSACTIONAL` use write-through caching, while `NONSTRICT_READ_WRITE` is a read-through caching strategy.
    For this reason, `NONSTRICT_READ_WRITE` is not very suitable if entities are changed frequently.

    </div>
    <div class="paragraph">

    When using clustering, the second-level cache entries are spread across multiple nodes.
    When using [Infinispan distributed cache](http://blog.infinispan.org/2015/10/hibernate-second-level-cache.html), only `READ_WRITE` and `NONSTRICT_READ_WRITE` are available for read-write caches.
    Bear in mind that `NONSTRICT_READ_WRITE` offers a weaker consistency guarantee since stale updates are possible.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    For more about Hibernate Performance Tuning, check out the [High-Performance Hibernate](https://www.youtube.com/watch?v=BTdTEe9QL5k&amp;t=1s) presentation from Devoxx France.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">