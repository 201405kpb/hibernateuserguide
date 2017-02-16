### 12.2. Session batching

    <div class="paragraph">

    The following example shows an anti-pattern for batch inserts.

    </div>
    <div id="batch-session-batch-example" class="exampleblock">
    <div class="title">Example 299. Naive way to insert 100 000 entities with Hibernate</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`EntityManager entityManager = null;
    EntityTransaction txn = null;
    try {
        entityManager = entityManagerFactory().createEntityManager();

        txn = entityManager.getTransaction();
        txn.begin();

        for ( int i = 0; i &lt; 100_000; i++ ) {
            Person Person = new Person( String.format( "Person %d", i ) );
            entityManager.persist( Person );
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
    <div class="paragraph">

    There are several problems associated with this example:

    </div>
    <div class="olist arabic">

1.  Hibernate caches all the newly inserted `Customer` instances in the session-level c1ache, so, when the transaction ends, 100 000 entities are managed by the persistence context.
    If the maximum memory allocated to the JVM is rather low, this example could fails with an `OutOfMemoryException`.
    The Java 1.8 JVM allocated either 1/4 of available RAM or 1Gb, which can easily accommodate 100 000 objects on the heap.
2.  long-running transactions can deplete a connection pool so other transactions don&#8217;t get a chance to proceed.
3.  JDBC batching is not enabled by default, so every insert statement requires a database roundtrip.
    To enable JDBC batching, set the `hibernate.jdbc.batch_size` property to an integer between 10 and 50.
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Hibernate disables insert batching at the JDBC level transparently if you use an identity identifier generator.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="sect3">