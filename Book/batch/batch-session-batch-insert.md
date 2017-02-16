#### 12.2.1. Batch inserts

    <div class="paragraph">

    When you make new objects persistent, employ methods `flush()` and `clear()` to the session regularly, to control the size of the first-level cache.

    </div>
    <div id="batch-session-batch-insert-example" class="exampleblock">
    <div class="title">Example 300. Flushing and clearing the `Session`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`EntityManager entityManager = null;
    EntityTransaction txn = null;
    try {
        entityManager = entityManagerFactory().createEntityManager();

        txn = entityManager.getTransaction();
        txn.begin();

        int batchSize = 25;

        for ( int i = 0; i &lt; entityCount; ++i ) {
            Person Person = new Person( String.format( "Person %d", i ) );
            entityManager.persist( Person );

            if ( i &gt; 0 &amp;&amp; i % batchSize == 0 ) {
                //flush a batch of inserts and release memory
                entityManager.flush();
                entityManager.clear();
            }
        }

        txn.commit();
    } catch (RuntimeException e) {
        if ( txn != null &amp;&amp; txn.isActive()) txn.rollback();
            throw e;
    } finally {
        if (entityManager != null) {
            entityManager.close();
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">