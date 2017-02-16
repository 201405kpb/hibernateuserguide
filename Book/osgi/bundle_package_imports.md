### 20.9. Bundle Package Imports

    <div class="paragraph">

    Your bundle&#8217;s manifest will need to import, at a minimum,

    </div>
    <div class="ulist">

*   `javax.persistence`
*   `org.hibernate.proxy` and `javassist.util.proxy`, due to Hibernate&#8217;s ability to return proxies for lazy initialization (Javassist enhancement occurs on the entity&#8217;s `ClassLoader` during runtime).
    </div>
    </div>
    <div class="sect2">