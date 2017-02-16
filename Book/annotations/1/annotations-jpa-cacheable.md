    #### 24.1.7. `@Cacheable`

    <div class="paragraph">

    The [`@Cacheable`](http://docs.oracle.com/javaee/7/api/javax/persistence/Cacheable.html) annotation is used to specify whether an entity should be stored in the second-level cache.

    </div>
    <div class="paragraph">

    If the `persistence.xml` `shared-cache-mode` XML attribute is set to `ENABLE_SELECTIVE`, then only the entities annotated with the `@Cacheable` are going to be stored in the second-level cache.

    </div>
    <div class="paragraph">

    If `shared-cache-mode` XML attribute value is `DISABLE_SELECTIVE`, then the entities marked with the `@Cacheable` annotation are not going to be stored in the second-level cache, while all the other entities are stored in the cache.

    </div>
    <div class="paragraph">

    See the [Caching](#caching) chapter for more info.

    </div>
    </div>
    <div class="sect3">