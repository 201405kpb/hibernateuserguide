### 10.7. The `buildLockRequest` API

    <div class="paragraph">

    Traditionally, Hibernate offered the `Session#lock()` method for acquiring an optimistic or a pessimistic lock on a given entity.
    Because varying the locking options was difficult when using a single `LockMode` parameter, Hibernate has added the `Session#buildLockRequest()` method API.

    </div>
    <div class="paragraph">

    The following example shows how to obtain shared database lock without waiting for the lock acquisition request.

    </div>
    <div id="locking-buildLockRequest-example" class="exampleblock">
    <div class="title">Example 279. `buildLockRequest` example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Person person = entityManager.find( Person.class, id );
    Session session = entityManager.unwrap( Session.class );
    session
        .buildLockRequest( LockOptions.NONE )
        .setLockMode( LockMode.PESSIMISTIC_READ )
        .setTimeOut( LockOptions.NO_WAIT )
        .lock( person );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT p.id AS id1_0_0_ ,
           p.name AS name2_0_0_
    FROM   Person p
    WHERE  p.id = 1

    SELECT id
    FROM   Person
    WHERE  id = 1
    FOR    SHARE NOWAIT`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">