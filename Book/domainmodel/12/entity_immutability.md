 #### 2.12.1. Entity immutability

    <div class="paragraph">

    If a specific entity is immutable, it is good practice to mark it with the `@Immutable` annotation.

    </div>
    <div class="exampleblock">
    <div class="title">Example 193. Immutable entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Event")
    @Immutable
    public static class Event {

        @Id
        private Long id;

        private Date createdOn;

        private String message;

        //Getters and setters are omitted for brevity

    }`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    Internally, Hibernate is going to perform several optimizations, such as:

    </div>
    <div class="ulist">

*   reducing memory footprint since there is no need to retain the dehydrated state for the dirty checking mechanism
*   speeding-up the Persistence Context flushing phase since immutable entities can skip the dirty checking process
    </div>
    <div class="paragraph">

    Considering the following entity is persisted in the database:

    </div>
    <div class="exampleblock">
    <div class="title">Example 194. Persisting an immutable entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Event event = new Event();
        event.setId( 1L );
        event.setCreatedOn( new Date( ) );
        event.setMessage( "Hibernate User Guide rocks!" );

        entityManager.persist( event );
    } );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    When loading the entity and trying to change its state,
    Hibernate will skip any modification, therefore no SQL `UPDATE` statement is executed.

    </div>
    <div class="exampleblock">
    <div class="title">Example 195. The immutable entity ignores any update</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Event event = entityManager.find( Event.class, 1L );
        log.info( "Change event message" );
        event.setMessage( "Hibernate User Guide" );
    } );
    doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Event event = entityManager.find( Event.class, 1L );
        assertEquals("Hibernate User Guide rocks!", event.getMessage());
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT e.id AS id1_0_0_,
           e.createdOn AS createdO2_0_0_,
           e.message AS message3_0_0_
    FROM   event e
    WHERE  e.id = 1

    -- Change event message

    SELECT e.id AS id1_0_0_,
           e.createdOn AS createdO2_0_0_,
           e.message AS message3_0_0_
    FROM   event e
    WHERE  e.id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect3">