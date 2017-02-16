    ### 20.13. Bundle Package Imports

    <div class="paragraph">

    Your bundle&#8217;s manifest will need to import, at a minimum,

    </div>
    <div class="ulist">

*   javax.persistence
*   `org.hibernate.proxy` and `javassist.util.proxy`, due to Hibernate&#8217;s ability to return proxies for lazy initialization (Javassist enhancement occurs on the entity&#8217;s `ClassLoader` during runtime)
*   JDBC driver package (example: `org.h2`)
*   `org.osgi.framework`, necessary to discover the `EntityManagerFactory` (described below)
    </div>
    </div>
    <div class="sect2">