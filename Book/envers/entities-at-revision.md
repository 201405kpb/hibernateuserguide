 ### 21.9. Querying for entities of a class at a given revision

    <div class="paragraph">

    The entry point for this type of queries is:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader()
        .createQuery()
        .forEntitiesAtRevision( MyEntity.class, revisionNumber );`</pre>
    </div>
    </div>
    <div class="paragraph">

    You can then specify constraints, which should be met by the entities returned, by adding restrictions, which can be obtained using the `AuditEntity` factory class.
    For example, to select only entities where the "name" property is equal to "John":

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`query.add( AuditEntity.property( "name" ).eq(  "John" ) );`</pre>
    </div>
    </div>
    <div class="paragraph">

    And to select only entities that are related to a given entity:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`query.add( AuditEntity.property( "address" ).eq( relatedEntityInstance ) );
    // or
    query.add( AuditEntity.relatedId( "address" ).eq( relatedEntityId ) );
    // or
    query.add( AuditEntity.relatedId( "address" ).in( relatedEntityId1, relatedEntityId2 ) );`</pre>
    </div>
    </div>
    <div class="paragraph">

    You can limit the number of results, order them, and set aggregations and projections (except grouping) in the usual way.
    When your query is complete, you can obtain the results by calling the `getSingleResult()` or `getResultList()` methods.

    </div>
    <div class="paragraph">

    A full query, can look for example like this:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List personsAtAddress = getAuditReader().createQuery()
        .forEntitiesAtRevision( Person.class, 12 )
        .addOrder( AuditEntity.property( "surname" ).desc() )
        .add( AuditEntity.relatedId( "address" ).eq( addressId ) )
        .setFirstResult( 4 )
        .setMaxResults( 2 )
        .getResultList();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">