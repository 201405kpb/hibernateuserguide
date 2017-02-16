 ### 13.2. Configuring second-level cache mappings

    <div class="paragraph">

    The cache mappings can be configured via JPA annotations or XML descriptors or using the Hibernate-specific mapping files.

    </div>
    <div class="paragraph">

    By default, entities are not part of the second level cache and we recommend you to stick to this setting.
    However, you can override this by setting the `shared-cache-mode` element in your `persistence.xml` file
    or by using the `javax.persistence.sharedCache.mode` property in your configuration file.
    The following values are possible:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`ENABLE_SELECTIVE` (Default and recommended value)</dt>
    <dd>

    Entities are not cached unless explicitly marked as cacheable (with the [`@Cacheable`](https://docs.oracle.com/javaee/7/api/javax/persistence/Cacheable.html) annotation).

    </dd>
    <dt class="hdlist1">`DISABLE_SELECTIVE`</dt>
    <dd>

    Entities are cached unless explicitly marked as non-cacheable.

    </dd>
    <dt class="hdlist1">`ALL`</dt>
    <dd>

    Entities are always cached even if marked as non-cacheable.

    </dd>
    <dt class="hdlist1">`NONE`</dt>
    <dd>

    No entity is cached even if marked as cacheable.
    This option can make sense to disable second-level cache altogether.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    The cache concurrency strategy used by default can be set globally via the `hibernate.cache.default_cache_concurrency_strategy` configuration property.
    The values for this property are:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">read-only</dt>
    <dd>

    If your application needs to read, but not modify, instances of a persistent class, a read-only cache is the best choice.
    Application can still delete entities and these changes should be reflected in second-level cache so that the cache
    does not provide stale entities.
    Implementations may use performance optimizations based on the immutability of entities.

    </dd>
    <dt class="hdlist1">read-write</dt>
    <dd>

    If the application needs to update data, a read-write cache might be appropriate.
    This strategy provides consistent access to single entity, but not a serializable transaction isolation level; e.g. when TX1 reads looks up an entity and does not find it, TX2 inserts the entity into cache and TX1 looks it up again, the new entity can be read in TX1.

    </dd>
    <dt class="hdlist1">nonstrict-read-write</dt>
    <dd>

    Similar to read-write strategy but there might be occasional stale reads upon concurrent access to an entity. The choice of this strategy might be appropriate if the application rarely updates the same data simultaneously and strict transaction isolation is not required. Implementations may use performance optimizations that make use of the relaxed consistency guarantee.

    </dd>
    <dt class="hdlist1">transactional</dt>
    <dd>

    Provides serializable transaction isolation level.

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

    Rather than using a global cache concurrency strategy, it is recommended to define this setting on a per entity basis.
    Use the [`@org.hibernate.annotations.Cache`](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/annotations/Cache.html) annotation for that.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The `@Cache` annotation define three attributes:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">usage</dt>
    <dd>

    Defines the `CacheConcurrencyStrategy`

    </dd>
    <dt class="hdlist1">region</dt>
    <dd>

    Defines a cache region where entries will be stored

    </dd>
    <dt class="hdlist1">include</dt>
    <dd>

    If lazy properties should be included in the second level cache.
    The default value is `all` so lazy properties are cacheable.
    The other possible value is `non-lazy` so lazy properties are not cacheable.

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">