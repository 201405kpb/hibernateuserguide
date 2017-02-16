 ### 13.5. Collection cache

    <div class="paragraph">

    Hibernate can also cache collections, and the `@Cache` annotation must be on added to the collection property.

    </div>
    <div class="paragraph">

    If the collection is made of value types (basic or embeddables mapped with `@ElementCollection`),
    the collection is stored as such.
    If the collection contains other entities (`@OneToMany` or `@ManyToMany`),
    the collection cache entry will store the entity identifiers only.

    </div>
    <div id="caching-collection-mapping-example" class="exampleblock">
    <div class="title">Example 316. Collection cache mapping</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "person", cascade = CascadeType.ALL)
    @org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.NONSTRICT_READ_WRITE)
    private List&lt;Phone&gt; phones = new ArrayList&lt;&gt;(  );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Collections are read-through, meaning they are cached upon being accessed for the first time:

    </div>
    <div id="caching-collection-example" class="exampleblock">
    <div class="title">Example 317. Collection cache usage</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, 1L );
    person.getPhones().size();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Subsequent collection retrievals will use the cache instead of going to the database.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    The collection cache is not write-through so any modification will trigger a collection cache entry invalidation.
    On a subsequent access, the collection will be loaded from the database and re-cached.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect2">