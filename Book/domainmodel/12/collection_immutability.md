#### 2.12.2. Collection immutability

    <div class="paragraph">

    Just like entities, collections can also be marked with the `@Immutable` annotation.

    </div>
    <div class="paragraph">

    Considering the following entity mappings:

    </div>
    <div class="exampleblock">
    <div class="title">Example 196. Immutable collection</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`@Entity(name = "Batch")
    public static class Batch {

        @Id
        private Long id;

        private String name;

        @OneToMany(cascade = CascadeType.ALL)
        @Immutable
        private List&lt;Event&gt; events = new ArrayList&lt;&gt;( );

        //Getters and setters are omitted for brevity

    }

    @Entity(name = "Event")
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

    This time, not only the `Event` entity is immutable, but the `Event` collection stored by the `Batch` parent entity.
    Once the immutable collection is created, it can never be modified.

    </div>
    <div class="exampleblock">
    <div class="title">Example 197. Persisting an immutable collection</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Batch batch = new Batch();
        batch.setId( 1L );
        batch.setName( "Change request" );

        Event event1 = new Event();
        event1.setId( 1L );
        event1.setCreatedOn( new Date( ) );
        event1.setMessage( "Update Hibernate User Guide" );

        Event event2 = new Event();
        event2.setId( 2L );
        event2.setCreatedOn( new Date( ) );
        event2.setMessage( "Update Hibernate Getting Started Guide" );

        batch.getEvents().add( event1 );
        batch.getEvents().add( event2 );

        entityManager.persist( batch );
    } );`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `Batch` entity is mutable. Only the `events` collection is immutable.

    </div>
    <div class="paragraph">

    For instance, we can still modify the entity name:

    </div>
    <div class="exampleblock">
    <div class="title">Example 198. Changing the mutable entity</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`doInJPA( this::entityManagerFactory, entityManager -&gt; {
        Batch batch = entityManager.find( Batch.class, 1L );
        log.info( "Change batch name" );
        batch.setName( "Proposed change request" );
    } );`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`SELECT b.id AS id1_0_0_,
           b.name AS name2_0_0_
    FROM   Batch b
    WHERE  b.id = 1

    -- Change batch name

    UPDATE batch
    SET    name = 'Proposed change request'
    WHERE  id = 1`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    However, when trying to modify the `events` collection:

    </div>
    <div class="exampleblock">
    <div class="title">Example 199. Immutable collections cannot be modified</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`try {
        doInJPA( this::entityManagerFactory, entityManager -&gt; {
            Batch batch = entityManager.find( Batch.class, 1L );
            batch.getEvents().clear();
        } );
    }
    catch ( Exception e ) {
        log.error( "Immutable collections cannot be modified" );
    }`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Unresolved directive in chapters/domain/immutability.adoc - include::extras/immutability/collection-immutability-update-example.log[]`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="admonitionblock tip">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    While immutable entity changes are simply discarded, modifying an immutable collection end up in a `HibernateException` being thrown.

    </div>
    </td>
    </tr>
    </table>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">