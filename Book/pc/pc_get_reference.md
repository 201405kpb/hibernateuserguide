 ### 5.5. Obtain an entity reference without initializing its data

    <div class="paragraph">

    Sometimes referred to as lazy loading, the ability to obtain a reference to an entity without having to load its data is hugely important.
    The most common case being the need to create an association between an entity and another existing entity.

    </div>
    <div id="pc-get-reference-jpa-example" class="exampleblock">
    <div class="title">Example 232. Obtaining an entity reference without initializing its data with JPA</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Book book = new Book();
    book.setAuthor( entityManager.getReference( Person.class, personId ) );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div id="pc-get-reference-native-example" class="exampleblock">
    <div class="title">Example 233. Obtaining an entity reference without initializing its data with Hibernate API</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Book book = new Book();
    book.setId( 1L );
    book.setIsbn( "123-456-7890" );
    entityManager.persist( book );
    book.setAuthor( session.load( Person.class, personId ) );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The above works on the assumption that the entity is defined to allow lazy loading, generally through use of runtime proxies.
    In both cases an exception will be thrown later if the given entity does not refer to actual database state when the application attempts to use the returned proxy in any way that requires access to its data.

    </div>
    </div>
    <div class="sect2">