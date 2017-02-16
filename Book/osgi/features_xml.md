 ### 20.3. features.xml

    <div class="paragraph">

    Apache Karaf environments tend to make heavy use of its "features" concept, where a feature is a set of order-specific bundles focused on a concise capability.
    These features are typically defined in a `features.xml` file.
    Hibernate produces and releases its own `features.xml` that defines a core `hibernate-orm`, as well as additional features for optional functionality (caching, Envers, etc.).
    This is included in the binary distribution, as well as deployed to the JBoss Nexus repository (using the `org.hibernate` groupId and `hibernate-osgi` with the `karaf.xml` classifier).

    </div>
    <div class="paragraph">

    Note that our features are versioned using the same ORM artifact versions they wrap.
    Also, note that the features are heavily tested against Karaf 3.0.3 as a part of our PaxExam-based integration tests.
    However, they&#8217;ll likely work on other versions as well.

    </div>
    <div class="paragraph">

    hibernate-osgi, theoretically, supports a variety of OSGi containers, such as Equinox.
    In that case, please use `features.xm`l as a reference for necessary bundles to activate and their correct ordering.
    However, note that Karaf starts a number of bundles automatically, several of which would need to be installed manually on alternatives.

    </div>
    </div>
    <div class="sect2">