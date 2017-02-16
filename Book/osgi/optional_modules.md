### 20.18. Optional Modules

    <div class="paragraph">

    The [unmanaged-native](https://github.com/hibernate/hibernate-demos/tree/master/hibernate-orm/osgi/unmanaged-native) demo project displays the use of optional Hibernate modules.
    Each module adds additional dependency bundles that must first be activated, either manually or through an additional feature.
    As of ORM 4.2, Envers is fully supported.
    Support for C3P0, Proxool, EhCache, and Infinispan were added in 4.3, however none of their 3rd party libraries currently work in OSGi (lots of `ClassLoader` problems, etc.).
    We&#8217;re tracking the issues in JIRA.

    </div>
    </div>
    <div class="sect2">