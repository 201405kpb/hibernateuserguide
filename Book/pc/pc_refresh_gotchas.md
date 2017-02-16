 #### 5.9.1. Refresh gotchas

    <div class="paragraph">

    The `refresh` entity state transition is meant to overwrite the entity attributes according to the info currently contained in the associated database record.

    </div>
    <div class="paragraph">

    However, you have to be very careful when cascading the refresh action to any transient entity.

    </div>
    <div class="paragraph">

    For instance, consider the following example:

    </div>
    <div id="pc-refresh-child-entity-jpa-example" class="exampleblock">
    <div class="title">Example 246. Refreshing entity state gotcha</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`try {
        Person person = entityManager.find( Person.class, personId );

        Book book = new Book();
        book.setId( 100L );
        book.setTitle( "Hibernate User Guide" );
        book.setAuthor( person );
        person.getBooks().add( book );

        entityManager.refresh( person );
    }
    catch ( EntityNotFoundException expected ) {
        log.info( "Beware when cascading the refresh associations to transient entities!" );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    In the aforementioned example, an `EntityNotFoundException` is thrown because the `Book` entity is still in a transient state.
    When the refresh action is cascaded from the `Person` entity, Hibernate will not be able to locate the `Book` entity in the database.

    </div>
    <div class="paragraph">

    For this reason, you should be very careful when mixing the refresh action with transient child entity objects.

    </div>
    </div>
    </div>
    <div class="sect2">