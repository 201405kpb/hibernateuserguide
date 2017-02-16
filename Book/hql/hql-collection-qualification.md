 ### 15.18. Special case - qualified path expressions

    <div class="paragraph">

    We said earlier that collection-valued associations actually refer to the _values_ of that collection.
    Based on the type of collection, there are also available a set of explicit qualification expressions.

    </div>
    <div id="hql-collection-qualification-example" class="exampleblock">
    <div class="title">Example 380. Qualified collection references example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@OneToMany(mappedBy = "phone")
    @MapKey(name = "timestamp")
    @MapKeyTemporal(TemporalType.TIMESTAMP )
    private Map&lt;Date, Call&gt; callHistory = new HashMap&lt;&gt;();

    // select all the calls (the map value) for a given Phone
    List&lt;Call&gt; calls = entityManager.createQuery(
        "select ch " +
        "from Phone ph " +
        "join ph.callHistory ch " +
        "where ph.id = :id ", Call.class )
    .setParameter( "id", id )
    .getResultList();

    // same as above
    List&lt;Call&gt; calls = entityManager.createQuery(
        "select value(ch) " +
        "from Phone ph " +
        "join ph.callHistory ch " +
        "where ph.id = :id ", Call.class )
    .setParameter( "id", id )
    .getResultList();

    // select all the Call timestamps (the map key) for a given Phone
    List&lt;Date&gt; timestamps = entityManager.createQuery(
        "select key(ch) " +
        "from Phone ph " +
        "join ph.callHistory ch " +
        "where ph.id = :id ", Date.class )
    .setParameter( "id", id )
    .getResultList();

    // select all the Call and their timestamps (the 'Map.Entry') for a given Phone
    List&lt;Map.Entry&lt;Date, Call&gt;&gt; callHistory = entityManager.createQuery(
        "select entry(ch) " +
        "from Phone ph " +
        "join ph.callHistory ch " +
        "where ph.id = :id " )
    .setParameter( "id", id )
    .getResultList();

    // Sum all call durations for a given Phone of a specific Person
    Long duration = entityManager.createQuery(
        "select sum(ch.duration) " +
        "from Person pr " +
        "join pr.phones ph " +
        "join ph.callHistory ch " +
        "where ph.id = :id " +
        "  and index(ph) = :phoneIndex", Long.class )
    .setParameter( "id", id )
    .setParameter( "phoneIndex", phoneIndex )
    .getSingleResult();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">VALUE</dt>
    <dd>

    Refers to the collection value.
    Same as not specifying a qualifier.
    Useful to explicitly show intent.
    Valid for any type of collection-valued reference.

    </dd>
    <dt class="hdlist1">INDEX</dt>
    <dd>

    According to HQL rules, this is valid for both `Maps` and `Lists` which specify a `javax.persistence.OrderColumn` annotation to refer to the `Map` key or the `List` position (aka the `OrderColumn` value).
    JPQL however, reserves this for use in the `List` case and adds `KEY` for the `Map` case.
    Applications interested in JPA provider portability should be aware of this distinction.

    </dd>
    <dt class="hdlist1">KEY</dt>
    <dd>

    Valid only for `Maps`. Refers to the map&#8217;s key. If the key is itself an entity, it can be further navigated.

    </dd>
    <dt class="hdlist1">ENTRY</dt>
    <dd>

    Only valid for `Maps`. Refers to the map&#8217;s logical `java.util.Map.Entry` tuple (the combination of its key and value).
    `ENTRY` is only valid as a terminal path and it&#8217;s applicable to the `SELECT` clause only.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    See [Collection-related expressions](#hql-collection-expressions) for additional details on collection related expressions.

    </div>
    </div>
    <div class="sect2">