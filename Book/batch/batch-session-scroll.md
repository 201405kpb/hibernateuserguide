 #### 12.2.2. Session scroll

    <div class="paragraph">

    When you retrieve and update data, `flush()` and `clear()` the session regularly.
    In addition, use method `scroll()` to take advantage of server-side cursors for queries that return many rows of data.

    </div>
    <div id="batch-session-scroll-example" class="exampleblock">
    <div class="title">Example 301. Using `scroll()`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`EntityManager entityManager = null;
    EntityTransaction txn = null;
    ScrollableResults scrollableResults = null;
    try {
        entityManager = entityManagerFactory().createEntityManager();

        txn = entityManager.getTransaction();
        txn.begin();

        int batchSize = 25;

        Session session = entityManager.unwrap( Session.class );

        scrollableResults = session
            .createQuery( "select p from Person p" )
            .setCacheMode( CacheMode.IGNORE )
            .scroll( ScrollMode.FORWARD_ONLY );

        int count = 0;
        while ( scrollableResults.next() ) {
            Person Person = (Person) scrollableResults.get( 0 );
            processPerson(Person);
            if ( ++count % batchSize == 0 ) {
                //flush a batch of updates and release memory:
                entityManager.flush();
                entityManager.clear();
            }
        }

        txn.commit();
    } catch (RuntimeException e) {
        if ( txn != null &amp;&amp; txn.isActive()) txn.rollback();
            throw e;
    } finally {
        if (scrollableResults != null) {
            scrollableResults.close();
        }
        if (entityManager != null) {
            entityManager.close();
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    If left unclosed by the application, Hibernate will automatically close the underlying resources (e.g. `ResultSet` and `PreparedStatement`) used internally by the `ScrollableResults` when the current transaction is ended (either commit or rollback).

    </div>
    <div class="paragraph">

    However, it is good practice to close the `ScrollableResults` explicitly.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    <div class="sect3">
