 ### 29.12. Queries by natural identifier

    <div class="paragraph">

    For most queries, including criteria queries, the query cache is not efficient because query cache invalidation occurs too frequently.
    However, there is a special kind of query where you can optimize the cache invalidation algorithm: lookups by a constant natural key.
    In some applications, this kind of query occurs frequently.
    The Criteria API provides special provision for this use case.

    </div>
    <div class="paragraph">

    First, map the natural key of your entity using `&lt;natural-id&gt;` and enable use of the second-level cache.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`&lt;class name="User"&gt;
        &lt;cache usage="read-write"/&gt;
        &lt;id name="id"&gt;
            &lt;generator class="increment"/&gt;
        &lt;/id&gt;
        &lt;natural-id&gt;
            &lt;property name="name"/&gt;
            &lt;property name="org"/&gt;
        &lt;/natural-id&gt;
        &lt;property name="password"/&gt;
    &lt;/class&gt;`</pre>
    </div>
    </div>
    <div class="paragraph">

    This functionality is not intended for use with entities with _mutable_ natural keys.

    </div>
    <div class="paragraph">

    Once you have enabled the Hibernate query cache, the `Restrictions.naturalId()` allows you to make use of the more efficient cache algorithm.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`session.createCriteria(User.class)
        .add( Restrictions.naturalId()
            .set("name", "gavin")
            .set("org", "hb")
        ).setCacheable(true)
        .uniqueResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">