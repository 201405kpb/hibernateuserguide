### 21.11. Querying for revisions of entity that modified given property

    <div class="paragraph">

    For the two types of queries described above it&#8217;s possible to use special `Audit` criteria called `hasChanged()` and `hasNotChanged()`
    that makes use of the functionality described in [Tracking entity changes at property level](#envers-tracking-properties-changes).
    They&#8217;re best suited for vertical queries, however existing API doesn&#8217;t restrict their usage for horizontal ones.

    </div>
    <div class="paragraph">

    Let&#8217;s have a look at following examples:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
    	.forRevisionsOfEntity( MyEntity.class, false, true )
    	.add( AuditEntity.id().eq( id ) );
    	.add( AuditEntity.property( "actualDate" ).hasChanged() );`</pre>
    </div>
    </div>
    <div class="paragraph">

    This query will return all revisions of `MyEntity` with given `id`, where the `actualDate` property has been changed.
    Using this query we won&#8217;t get all other revisions in which `actualDate` wasn&#8217;t touched.
    Of course, nothing prevents user from combining `hasChanged` condition with some additional criteria - add method can be used here in a normal way.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
    	.forEntitiesAtRevision( MyEntity.class, revisionNumber )
    	.add( AuditEntity.property( "prop1" ).hasChanged() )
    	.add( AuditEntity.property( "prop2" ).hasNotChanged() );`</pre>
    </div>
    </div>
    <div class="paragraph">

    This query will return horizontal slice for `MyEntity` at the time `revisionNumber` was generated.
    It will be limited to revisions that modified `prop1` but not `prop2`.

    </div>
    <div class="paragraph">

    Note that the result set will usually also contain revisions with numbers lower than the `revisionNumber`,
    so wem cannot read this query as "Give me all MyEntities changed in `revisionNumber` with `prop1` modified and `prop2` untouched".
    To get such result we have to use the `forEntitiesModifiedAtRevision` query:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
    	.forEntitiesModifiedAtRevision( MyEntity.class, revisionNumber )
    	.add( AuditEntity.property( "prop1" ).hasChanged() )
    	.add( AuditEntity.property( "prop2" ).hasNotChanged() );`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">