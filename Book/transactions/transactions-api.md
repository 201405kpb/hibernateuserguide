 ### 8.3. Hibernate Transaction API

    <div class="paragraph">

    Hibernate provides an API for helping to isolate applications from the differences in the underlying physical transaction system in use.
    Based on the configured `TransactionCoordinatorBuilder`, Hibernate will simply do the right thing when this transaction API is used by the application.
    This allows your applications and components to be more portable move around into different environments.

    </div>
    <div class="paragraph">

    To use this API, you would obtain the `org.hibernate.Transaction` from the Session. `Transaction` allows for all the normal operations you&#8217;d expect: `begin`, `commit` and `rollback`, and it even exposes some cool methods like:

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`markRollbackOnly`</dt>
    <dd>

    that works in both JTA and JDBC

    </dd>
    <dt class="hdlist1">`getTimeout` and `setTimeout`</dt>
    <dd>

    that again work in both JTA and JDBC

    </dd>
    <dt class="hdlist1">`registerSynchronization`</dt>
    <dd>

    that allows you to register JTA Synchronizations even in non-JTA environments.
    In fact in both JTA and JDBC environments, these `Synchronizations` are kept locally by Hibernate.
    In JTA environments, Hibernate will only ever register one single `Synchronization` with the `TransactionManager` to avoid ordering problems.

    </dd>
    </dl>
    </div>
    <div class="paragraph">

    Additionally, it exposes a getStatus method that returns an `org.hibernate.resource.transaction.spi.TransactionStatus` enum.
    This method checks with the underlying transaction system if needed, so care should be taken to minimize its use; it can have a big performance impact in certain JTA set ups.

    </div>
    <div class="paragraph">

    Let&#8217;s take a look at using the Transaction API in the various environments.

    </div>
    <div id="transactions-api-jdbc-example" class="exampleblock">
    <div class="title">Example 273. Using Transaction API in JDBC</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StandardServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder()
            // "jdbc" is the default, but for explicitness
            .applySetting( AvailableSettings.TRANSACTION_COORDINATOR_STRATEGY, "jdbc" )
            .build();

    Metadata metadata = new MetadataSources( serviceRegistry )
            .addAnnotatedClass( Customer.class )
            .getMetadataBuilder()
            .build();

    SessionFactory sessionFactory = metadata.getSessionFactoryBuilder()
            .build();

    Session session = sessionFactory.openSession();
    try {
        // calls Connection#setAutoCommit( false ) to
        // signal start of transaction
        session.getTransaction().begin();

        session.createQuery( "UPDATE customer set NAME = 'Sir. '||NAME" )
                .executeUpdate();

        // calls Connection#commit(), if an error
        // happens we attempt a rollback
        session.getTransaction().commit();
    }
    catch ( Exception e ) {
        // we may need to rollback depending on
        // where the exception happened
        if ( session.getTransaction().getStatus() == TransactionStatus.ACTIVE
                || session.getTransaction().getStatus() == TransactionStatus.MARKED_ROLLBACK ) {
            session.getTransaction().rollback();
        }
        // handle the underlying error
    }
    finally {
        session.close();
        sessionFactory.close();
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="transactions-api-cmt-example" class="exampleblock">
    <div class="title">Example 274. Using Transaction API in JTA (CMT)</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StandardServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder()
            // "jdbc" is the default, but for explicitness
            .applySetting( AvailableSettings.TRANSACTION_COORDINATOR_STRATEGY, "jta" )
            .build();

    Metadata metadata = new MetadataSources( serviceRegistry )
            .addAnnotatedClass( Customer.class )
            .getMetadataBuilder()
            .build();

    SessionFactory sessionFactory = metadata.getSessionFactoryBuilder()
            .build();

    // Note: depending on the JtaPlatform used and some optional settings,
    // the underlying transactions here will be controlled through either
    // the JTA TransactionManager or UserTransaction

    Session session = sessionFactory.openSession();
    try {
        // Since we are in CMT, a JTA transaction would
        // already have been started.  This call essentially
        // no-ops
        session.getTransaction().begin();

        Number customerCount = (Number) session.createQuery( "select count(c) from Customer c" ).uniqueResult();

        // Since we did not start the transaction ( CMT ),
        // we also will not end it.  This call essentially
        // no-ops in terms of transaction handling.
        session.getTransaction().commit();
    }
    catch ( Exception e ) {
        // again, the rollback call here would no-op (aside from
        // marking the underlying CMT transaction for rollback only).
        if ( session.getTransaction().getStatus() == TransactionStatus.ACTIVE
                || session.getTransaction().getStatus() == TransactionStatus.MARKED_ROLLBACK ) {
            session.getTransaction().rollback();
        }
        // handle the underlying error
    }
    finally {
        session.close();
        sessionFactory.close();
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="transactions-api-bmt-example" class="exampleblock">
    <div class="title">Example 275. Using Transaction API in JTA (BMT)</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`StandardServiceRegistry serviceRegistry = new StandardServiceRegistryBuilder()
            // "jdbc" is the default, but for explicitness
            .applySetting( AvailableSettings.TRANSACTION_COORDINATOR_STRATEGY, "jta" )
            .build();

    Metadata metadata = new MetadataSources( serviceRegistry )
            .addAnnotatedClass( Customer.class )
            .getMetadataBuilder()
            .build();

    SessionFactory sessionFactory = metadata.getSessionFactoryBuilder()
            .build();

    // Note: depending on the JtaPlatform used and some optional settings,
    // the underlying transactions here will be controlled through either
    // the JTA TransactionManager or UserTransaction

    Session session = sessionFactory.openSession();
    try {
        // Assuming a JTA transaction is not already active,
        // this call the TM/UT begin method.  If a JTA
        // transaction is already active, we remember that
        // the Transaction associated with the Session did
        // not "initiate" the JTA transaction and will later
        // nop-op the commit and rollback calls...
        session.getTransaction().begin();

        session.persist( new Customer(  ) );
        Customer customer = (Customer) session.createQuery( "select c from Customer c" ).uniqueResult();

        // calls TM/UT commit method, assuming we are initiator.
        session.getTransaction().commit();
    }
    catch ( Exception e ) {
        // we may need to rollback depending on
        // where the exception happened
        if ( session.getTransaction().getStatus() == TransactionStatus.ACTIVE
                || session.getTransaction().getStatus() == TransactionStatus.MARKED_ROLLBACK ) {
            // calls TM/UT commit method, assuming we are initiator;
            // otherwise marks the JTA transaction for rollback only
            session.getTransaction().rollback();
        }
        // handle the underlying error
    }
    finally {
        session.close();
        sessionFactory.close();
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the CMT case we really could have omitted all of the Transaction calls.
    But the point of the examples was to show that the Transaction API really does insulate your code from the underlying transaction mechanism.
    In fact, if you strip away the comments and the single configuration setting supplied at bootstrap, the code is exactly the same in all 3 examples.
    In other words, we could develop that code and drop it, as-is, in any of the 3 transaction environments.

    </div>
    <div class="paragraph">

    The Transaction API tries hard to make the experience consistent across all environments.
    To that end, it generally defers to the JTA specification when there are differences (for example automatically trying rollback on a failed commit).

    </div>
    </div>
    <div class="sect2">