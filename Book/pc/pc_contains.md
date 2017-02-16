### 5.11. Checking persistent state

    <div class="paragraph">

    An application can verify the state of entities and collections in relation to the persistence context.

    </div>
    <div id="pc-contains-jpa-example" class="exampleblock">
    <div class="title">Example 252. Verifying managed state with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`boolean contained = entityManager.contains( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-contains-native-example" class="exampleblock">
    <div class="title">Example 253. Verifying managed state with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`boolean contained = session.contains( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-verify-lazy-jpa-example" class="exampleblock">
    <div class="title">Example 254. Verifying laziness with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`PersistenceUnitUtil persistenceUnitUtil = entityManager.getEntityManagerFactory().getPersistenceUnitUtil();

    boolean personInitialized = persistenceUnitUtil.isLoaded( person );

    boolean personBooksInitialized = persistenceUnitUtil.isLoaded( person.getBooks() );

    boolean personNameInitialized = persistenceUnitUtil.isLoaded( person, "name" );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-verify-lazy-native-example" class="exampleblock">
    <div class="title">Example 255. Verifying laziness with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`boolean personInitialized = Hibernate.isInitialized( person );

    boolean personBooksInitialized = Hibernate.isInitialized( person.getBooks() );

    boolean personNameInitialized = Hibernate.isPropertyInitialized( person, "name" );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In JPA there is an alternative means to check laziness using the following `javax.persistence.PersistenceUtil` pattern (which is recommended wherever possible).

    </div>
    <div id="pc-verify-lazy-jpa-alternative-example" class="exampleblock">
    <div class="title">Example 256. Alternative JPA means to verify laziness</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`PersistenceUtil persistenceUnitUtil = Persistence.getPersistenceUtil();

    boolean personInitialized = persistenceUnitUtil.isLoaded( person );

    boolean personBooksInitialized = persistenceUnitUtil.isLoaded( person.getBooks() );

    boolean personNameInitialized = persistenceUnitUtil.isLoaded( person, "name" );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">