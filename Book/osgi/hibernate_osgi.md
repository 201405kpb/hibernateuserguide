### 20.2. hibernate-osgi

<div class="paragraph">

Rather than embed OSGi capabilities into hibernate-core, and sub-modules, hibernate-osgi was created.
It&#8217;s purposefully separated, isolating all OSGi dependencies.
It provides an OSGi-specific `ClassLoader` (aggregates the container&#8217;s `ClassLoader` with core and `EntityManager` `ClassLoader`s),
JPA persistence provider, `SessionFactory`/`EntityManagerFactory` bootstrapping, entities/mappings scanner, and service management.

</div>
</div>
<div class="sect2">