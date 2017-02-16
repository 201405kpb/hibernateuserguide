### 15.54. Read-only entities

    <div class="paragraph">

    As explained in [entity immutability](#entity-immutability) section, fetching entities in read-only mode is much more efficient than fetching read-write entities.
    Even if the entities are mutable, you can still fetch them in read-only mode, and benefit from reducing the memory footprint and sepeding up the flushing process.

    </div>
    <div class="paragraph">

    Read-only entities are skipped by the dirty checking mechanism as illustrated by the following example:

    </div>
    <div id="hql-read-only-entities-example" class="exampleblock">
    <div class="title">Example 407. Read-only entities query example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "join c.phone p " +
        "where p.number = :phoneNumber ", Call.class )
    .setParameter( "phoneNumber", "123-456-7890" )
    .setHint( "org.hibernate.readOnly", true )
    .getResultList();

    calls.forEach( c -&gt; c.setDuration( 0 ) );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT c.id AS id1_5_ ,
           c.duration AS duration2_5_ ,
           c.phone_id AS phone_id4_5_ ,
           c.call_timestamp AS call_tim3_5_
    FROM   phone_call c
    INNER JOIN phone p ON c.phone_id = p.id
    WHERE   p.phone_number = '123-456-7890'`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    As you can see, there is no SQL `UPDATE` being executed.

    </div>
    <div class="paragraph">

    The Hibernate native API offers a `Query#setReadOnly` method, as an alternative to using a JPA query hint:

    </div>
    <div id="hql-read-only-entities-native-example" class="exampleblock">
    <div class="title">Example 408. Read-only entities native query example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Call&gt; calls = entityManager.createQuery(
        "select c " +
        "from Call c " +
        "join c.phone p " +
        "where p.number = :phoneNumber ", Call.class )
    .setParameter( "phoneNumber", "123-456-7890" )
    .unwrap( org.hibernate.query.Query.class )
    .setReadOnly( true )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">