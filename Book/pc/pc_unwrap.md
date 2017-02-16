 ### 5.1. Accessing Hibernate APIs from JPA

    <div class="paragraph">

    JPA defines an incredibly useful method to allow applications access to the APIs of the underlying provider.

    </div>
    <div id="pc-unwrap-example" class="exampleblock">
    <div class="title">Example 221. Accessing Hibernate APIs from JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = entityManager.unwrap( Session.class );
    SessionImplementor sessionImplementor = entityManager.unwrap( SessionImplementor.class );

    SessionFactory sessionFactory = entityManager.getEntityManagerFactory().unwrap( SessionFactory.class );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">