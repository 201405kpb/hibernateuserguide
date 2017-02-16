 ### 21.10. Querying for revisions, at which entities of a given class changed

    <div class="paragraph">

    The entry point for this type of queries is:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
        .forRevisionsOfEntity( MyEntity.class, false, true );`</pre>
    </div>
    </div>
    <div class="paragraph">

    You can add constraints to this query in the same way as to the previous one.
    There are some additional possibilities:

    </div>
    <div class="olist arabic">

1.  using `AuditEntity.revisionNumber()` you can specify constraints, projections and order on the revision number, in which the audited entity was modified
2.  similarly, using `AuditEntity.revisionProperty( propertyName )` you can specify constraints, projections and order on a property of the revision entity,
    corresponding to the revision in which the audited entity was modified
3.  `AuditEntity.revisionType()` gives you access as above to the type of the revision (`ADD`, `MOD`, `DEL`).
    </div>
    <div class="paragraph">

    Using these methods, you can order the query results by revision number, set projection or constraint the revision number to be greater or less than a specified value, etc.
    For example, the following query will select the smallest revision number, at which entity of class `MyEntity` with id `entityId` has changed, after revision number 42:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Number revision = (Number) getAuditReader().createQuery()
        .forRevisionsOfEntity( MyEntity.class, false, true )
        .setProjection( AuditEntity.revisionNumber().min() )
        .add( AuditEntity.id().eq( entityId ) )
        .add( AuditEntity.revisionNumber().gt( 42 ) )
        .getSingleResult();`</pre>
    </div>
    </div>
    <div class="paragraph">

    The second additional feature you can use in queries for revisions is the ability to _maximize_/_minimize_ a property.
    For example, if you want to select the smallest possibler revision at which the value of the `actualDate` for a given entity was larger then a given value:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Number revision = (Number) getAuditReader().createQuery()
    	.forRevisionsOfEntity( MyEntity.class, false, true) // We are only interested in the first revision
    	.setProjection( AuditEntity.revisionNumber().min() )
    	.add( AuditEntity.property( "actualDate" ).minimize()
    	.add( AuditEntity.property( "actualDate" ).ge( givenDate ) )
    	.add( AuditEntity.id().eq( givenEntityId ) )) .getSingleResult();`</pre>
    </div>
    </div>
    <div class="paragraph">

    The `minimize()` and `maximize()` methods return a criteria, to which you can add constraints, which must be met by the entities with the _maximized_/_minimized_ properties.

    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    `AggregatedAuditExpression#computeAggregationInInstanceContext()` enables the possibility to compute aggregated expression in the context of each entity instance separately.
    It turns out useful when querying for latest revisions of all entities of a particular type.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    You probably also noticed that there are two boolean parameters, passed when creating the query.

    </div>
    <div class="dlist">
    <dl>
    <dt class="hdlist1">`selectEntitiesOnly`</dt>
    <dd>

    the first parameter is only valid when you don&#8217;t set an explicit projection.
    If true, the result of the query will be a list of entities (which changed at revisions satisfying the specified constraints).
    If false, the result will be a list of three element arrays:

    <div class="ulist">

*   the first element will be the changed entity instance.
*   the second will be an entity containing revision data (if no custom entity is used, this will be an instance of `DefaultRevisionEntity`).
*   the third will be the type of the revision (one of the values of the `RevisionType` enumeration: `ADD`, `MOD`, `DEL`).
    </div>
    </dd>
    <dt class="hdlist1">`selectDeletedEntities`</dt>
    <dd>

    the second parameter specifies if revisions, in which the entity was deleted should be included in the results.
    If yes, such entities will have the revision type `DEL` and all fields, except the id, `null`.

    </dd>
    </dl>
    </div>
    </div>
    <div class="sect2">