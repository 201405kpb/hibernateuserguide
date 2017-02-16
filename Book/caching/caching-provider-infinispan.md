### 13.11. Infinispan

    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Use of the build-in integration for [Infinispan](http://infinispan.org/) requires that the `hibernate-infinispan module` jar (and all of its dependencies) are on the classpath.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Infinispan currently supports all cache concurrency modes, although not all combinations of configurations are compatible.

    </div>
    <div class="paragraph">

    Traditionally the `transactional` and `read-only` strategy was supported on _transactional invalidation_ caches. In version 5.0, further modes have been added:

    </div>
    <div class="ulist">

*   _non-transactional invalidation_ caches are supported as well with `read-write` strategy. The actual setting of cache concurrency mode (`read-write` vs. `transactional`) is not honored, the appropriate strategy is selected based on the cache configuration (_non-transactional_ vs. _transactional_).
*   `read-write` mode is supported on _non-transactional distributed/replicated_ caches, however, eviction should not be used in this configuration. Use of eviction can lead to consistency issues. Expiration (with reasonably long max-idle times) can be used.
*   `nonstrict-read-write` mode is supported on _non-transactional distributed/replicated_ caches, but the eviction should be turned off as well. In addition to that, the entities must use versioning. This mode mildly relaxes the consistency - between DB commit and end of transaction commit a stale read (see [example](#caching-provider-infinispan-stale-read-example)) may occur in another transaction. However this strategy uses less RPCs and can be more performant than the other ones.
*   `read-only` mode is supported on both _transactional_ and _non-transactional_ _invalidation_ caches and _non-transactional distributed/replicated_ caches, but use of this mode currently does not bring any performance gains.
    </div>
    <div class="paragraph">

    The available combinations are summarized in table below:

    </div>
    <table id="caching-provider-infinispan-compatibility-table" class="tableblock frame-all grid-all spread">
    <caption class="title">Table 6. Cache concurrency strategy/cache mode compatibility table</caption>
    <colgroup>
    <col style="width: 25%;">
    <col style="width: 25%;">
    <col style="width: 25%;">
    <col style="width: 25%;">
    </colgroup>
    <thead>
    <tr>
    <th class="tableblock halign-left valign-top">Concurrency strategy</th>
    <th class="tableblock halign-left valign-top">Cache transactions</th>
    <th class="tableblock halign-left valign-top">Cache mode</th>
    <th class="tableblock halign-left valign-top">Eviction</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td class="tableblock halign-left valign-top">

    transactional
    </td>
    <td class="tableblock halign-left valign-top">

    transactional
    </td>
    <td class="tableblock halign-left valign-top">

    invalidation
    </td>
    <td class="tableblock halign-left valign-top">

    yes
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    read-write
    </td>
    <td class="tableblock halign-left valign-top">

    non-transactional
    </td>
    <td class="tableblock halign-left valign-top">

    invalidation
    </td>
    <td class="tableblock halign-left valign-top">

    yes
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    read-write
    </td>
    <td class="tableblock halign-left valign-top">

    non-transactional
    </td>
    <td class="tableblock halign-left valign-top">

    distributed/replicated
    </td>
    <td class="tableblock halign-left valign-top">

    no
    </td>
    </tr>
    <tr>
    <td class="tableblock halign-left valign-top">

    nonstrict-read-write
    </td>
    <td class="tableblock halign-left valign-top">

    non-transactional
    </td>
    <td class="tableblock halign-left valign-top">

    distributed/replicated
    </td>
    <td class="tableblock halign-left valign-top">

    no
    </td>
    </tr>
    </tbody>
    </table>
    <div class="paragraph">

    If your second level cache is not clustered, it is possible to use local cache instead of the clustered caches in all modes as described above.

    </div>
    <div id="caching-provider-infinispan-stale-read-example" class="exampleblock">
    <div class="title">Example 336. Stale read with `nonstrict-read-write` strategy</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`A=0 (non-cached), B=0 (cached in 2LC)
    TX1: write A = 1, write B = 1
    TX1: start commit
    TX1: commit A, B in DB
    TX2: read A = 1 (from DB), read B = 0 (from 2LC) // breaks transactional atomicity
    TX1: update A, B in 2LC
    TX1: end commit
    Tx3: read A = 1, B = 1 // reads after TX1 commit completes are consistent again`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">