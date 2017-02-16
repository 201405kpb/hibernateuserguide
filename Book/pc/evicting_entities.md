### 5.12. Evicting entities

    <div class="paragraph">

    When the `flush()` method is called, the state of the entity is synchronized with the database.
    If you do not want this synchronization to occur, or if you are processing a huge number of objects and need to manage memory efficiently,
    the `evict()` method can be used to remove the object and its collections from the first-level cache.

    </div>
    <div id="caching-management-jpa-detach-example" class="exampleblock">
    <div class="title">Example 257. Detaching an entity from the `EntityManager`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`for(Person person : entityManager.createQuery("select p from Person p", Person.class)
            .getResultList()) {
        dtos.add(toDTO(person));
        entityManager.detach( person );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="caching-management-native-evict-example" class="exampleblock">
    <div class="title">Example 258. Evicting an entity from the Hibernate `Session`</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Session session = entityManager.unwrap( Session.class );
    for(Person person : (List&lt;Person&gt;) session.createQuery("select p from Person p").list()) {
        dtos.add(toDTO(person));
        session.evict( person );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To detach all entities from the current persistence context, both the `EntityManager` and the Hibernate `Session` define a `clear()` method.

    </div>
    <div id="caching-management-clear-example" class="exampleblock">
    <div class="title">Example 259. Clearing the persistence context</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`entityManager.clear();

    session.clear();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    To verify if an entity instance is currently attached to the running persistence context, both the `EntityManager` and the Hibernate `Session` define a `contains(Object entity)` method.

    </div>
    <div id="caching-management-contains-example" class="exampleblock">
    <div class="title">Example 260. Verify if an entity is contained in a persistence context</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`entityManager.contains( person );

    session.contains( person );`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">