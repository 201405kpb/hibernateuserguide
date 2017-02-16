 ### 15.17. Collection member references

    <div class="paragraph">

    References to collection-valued associations actually refer to the _values_ of that collection.

    </div>
    <div id="hql-collection-valued-associations-example" class="exampleblock">
    <div class="title">Example 379. Collection references example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Phone&gt; phones = entityManager.createQuery(
        "select ph " +
        "from Person pr " +
        "join pr.phones ph " +
        "join ph.calls c " +
        "where pr.address = :address " +
        "  and c.duration &gt; :duration", Phone.class )
    .setParameter( "address", address )
    .setParameter( "duration", duration )
    .getResultList();

    // alternate syntax
    List&lt;Phone&gt; phones = session.createQuery(
        "select pr " +
        "from Person pr, " +
        "in (pr.phones) ph, " +
        "in (ph.calls) c " +
        "where pr.address = :address " +
        "  and c.duration &gt; :duration" )
    .setParameter( "address", address )
    .setParameter( "duration", duration )
    .list();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the example, the identification variable `ph` actually refers to the object model type `Phone`, which is the type of the elements of the `Person#phones` association.

    </div>
    <div class="paragraph">

    The example also shows the alternate syntax for specifying collection association joins using the `IN` syntax.
    Both forms are equivalent.
    Which form an application chooses to use is simply a matter of taste.

    </div>
    </div>
    <div class="sect2">