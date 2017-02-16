 ### 19.4. Multitenancy in Hibernate

    <div class="paragraph">

    Using Hibernate with multitenant data comes down to both an API and then integration piece(s).
    As usual, Hibernate strives to keep the API simple and isolated from any underlying integration complexities.
    The API is really just defined by passing the tenant identifier as part of opening any session.

    </div>
    <div id="multitenacy-hibernate-session-example" class="exampleblock">
    <div class="title">Example 486. Specifying tenant identifier from `SessionFactory`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`private void doInSession(String tenant, Consumer&lt;Session&gt; function) {
        Session session = null;
        Transaction txn = null;
        try {
            session = sessionFactory
                .withOptions()
                .tenantIdentifier( tenant )
                .openSession();
            txn = session.getTransaction();
            txn.begin();
            function.accept(session);
            txn.commit();
        } catch (Throwable e) {
            if ( txn != null ) txn.rollback();
            throw e;
        } finally {
            if (session != null) {
                session.close();
            }
        }
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Additionally, when specifying the configuration, an `org.hibernate.MultiTenancyStrategy` should be named using the `hibernate.multiTenancy` setting.
    Hibernate will perform validations based on the type of strategy you specify.
    The strategy here correlates to the isolation approach discussed above.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">NONE</dt>
    <dd>

    (the default) No multitenancy is expected.
    In fact, it is considered an error if a tenant identifier is specified when opening a session using this strategy.

    </dd>
    <dt class="hdlist1">SCHEMA</dt>
    <dd>

    Correlates to the separate schema approach.
    It is an error to attempt to open a session without a tenant identifier using this strategy.
    Additionally, a `MultiTenantConnectionProvider` must be specified.

    </dd>
    <dt class="hdlist1">DATABASE</dt>
    <dd>

    Correlates to the separate database approach.
    It is an error to attempt to open a session without a tenant identifier using this strategy.
    Additionally, a `MultiTenantConnectionProvider` must be specified.

    </dd>
    <dt class="hdlist1">DISCRIMINATOR</dt>
    <dd>

    Correlates to the partitioned (discriminator) approach.
    It is an error to attempt to open a session without a tenant identifier using this strategy.
    This strategy is not yet implemented and you can follow its progress via the [HHH-6054 Jira issue](https://hibernate.atlassian.net/browse/HHH-6054).

    </dd>
    </dl>
    </div>
    <div class="sect3">