 #### 12.2.3. StatelessSession

    <div class="paragraph">

    `StatelessSession` is a command-oriented API provided by Hibernate.
    Use it to stream data to and from the database in the form of detached objects.
    A `StatelessSession` has no persistence context associated with it and does not provide many of the higher-level life cycle semantics.

    </div>
    <div class="paragraph">

    Some of the things not provided by a `StatelessSession` include:

    </div>
    <div class="ulist">

*   a first-level cache
*   interaction with any second-level or query cache
*   transactional write-behind or automatic dirty checking
    </div>
    <div class="paragraph">

    Limitations of `StatelessSession`:

    </div>
    <div class="ulist">

*   Operations performed using a stateless session never cascade to associated instances.
*   Collections are ignored by a stateless session.
*   Lazy loading of associations is not supported.
*   Operations performed via a stateless session bypass Hibernate&#8217;s event model and interceptors.
*   Due to the lack of a first-level cache, Stateless sessions are vulnerable to data aliasing effects.
*   A stateless session is a lower-level abstraction that is much closer to the underlying JDBC.
    </div>
    <div id="batch-stateless-session-example" class="exampleblock">
    <div class="title">Example 302. Using a  `StatelessSession`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StatelessSession statelessSession = null;
    Transaction txn = null;
    ScrollableResults scrollableResults = null;
    try {
        SessionFactory sessionFactory = entityManagerFactory().unwrap( SessionFactory.class );
        statelessSession = sessionFactory.openStatelessSession();

        txn = statelessSession.getTransaction();
        txn.begin();

        scrollableResults = statelessSession
            .createQuery( "select p from Person p" )
            .scroll(ScrollMode.FORWARD_ONLY);

        while ( scrollableResults.next() ) {
            Person Person = (Person) scrollableResults.get( 0 );
            processPerson(Person);
            statelessSession.update( Person );
        }

        txn.commit();
    } catch (RuntimeException e) {
        if ( txn != null &amp;&amp; txn.getStatus() == TransactionStatus.ACTIVE) txn.rollback();
            throw e;
    } finally {
        if (scrollableResults != null) {
            scrollableResults.close();
        }
        if (statelessSession != null) {
            statelessSession.close();
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `Customer` instances returned by the query are immediately detached.
    They are never associated with any persistence context.

    </div>
    <div class="paragraph">

    The `insert()`, `update()`, and `delete()` operations defined by the `StatelessSession` interface operate directly on database rows.
    They cause the corresponding SQL operations to be executed immediately.
    They have different semantics from the `save()`, `saveOrUpdate()`, and `delete()` operations defined by the `Session` interface.

    </div>
    </div>
    </div>
    <div class="sect2">