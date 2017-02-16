### 21.13. Querying for entities using entity relation joins

    <div class="paragraph">

    Audit queries support the ability to apply constraints, projections, and sort operations based on entity relations.  In order
    to traverse entity relations through an audit query, you must use the relation traversal API with a join type.

    </div>
    <div class="admonitionblock important">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Relation join queries are considered experimental and may change in future releases.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Relation joins can only be applied to `*-to-one` mappings and can only be specified using `JoinType.LEFT` or
    `JoinType.INNER`.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    The basis for creating an entity relation join query is as follows:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`// create an inner join query
    AuditQuery query = getAuditReader().createQuery()
        .forEntitiesAtRevision( Car.class, 1 )
        .traverseRelation( "owner", JoinType.INNER );

    // create a left join query
    AuditQuery query = getAuditReader().createQuery()
        .forEntitiesAtRevision( Car.class, 1 )
        .traverseRelation( "owner", JoinType.LEFT );`</pre>
    </div>
    </div>
    <div class="paragraph">

    Like any other query, constraints may be added to restrict the results.  For example, to find all `Car` entities that
    have an owner with a name starting with `Joe`, you would use:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
        .forEntitiesAtRevision( Car.class, 1 )
        .traverseRelation( "owner", JoinType.INNER )
        .add( AuditEntity.property( "name" ).like( "Joe%" ) );`</pre>
    </div>
    </div>
    <div class="paragraph">

    It is also possible to traverse beyond the first relation in an entity graph.  For example, to find all `Car` entities
    where the owner&#8217;s address has a street number that equals `1234`:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
        .forEntitiesAtRevision( Car.class, 1 )
        .traverseRelation( "owner", JoinType.INNER )
        .traverseRelation( "address", JoinType.INNER )
        .add( AuditEntity.property( "streetNumber" ).eq( 1234 ) );`</pre>
    </div>
    </div>
    <div class="paragraph">

    Complex constraints may also be added that are applicable to properties of nested relations or the base query entity or
    relation state, such as testing for `null`.  For example, the following query illustrates how to find all `Car` entities where
    the owner&#8217;s age is `20` or that the car has _no_ owner:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
        .forEntitiesAtRevision( Car.class, 1 )
        .traverseRelation( "owner", JoinType.LEFT, "p" )
        .up()
        .add(
            AuditEntity.or(
                AuditEntity.property( "p", "age" ).eq( 20 ),
                AuditEntity.relatedId( "owner" ).eq( null )
            )
        )
        .addOrder( AuditEntity.property( "make" ).asc() );`</pre>
    </div>
    </div>
    <div class="admonitionblock note">
    <table>
    <tr>
    <td class="icon">

    </td>
    <td class="content">
    <div class="paragraph">

    Queries can use the `up` method to navigate back up the entity graph.

    </div>
    </td>
    </tr>
    </table>
    </div>
    <div class="paragraph">

    Disjunction criterion may also be applied to relation join queries.  For example, the following query will find all
    `Car` entities where the owner&#8217;s age is `20` or that the owner lives at an address where the street number equals `1234`:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
        .forEntitiesAtRevision( Car.class, 1 )
        .traverseRelation( "owner", JoinType.INNER, "p" )
        .traverseRelation( "address", JoinType.INNER, "a" )
        .up()
        .up()
        .add(
            AuditEntity.disjunction()
                .add( AuditEntity.property( "p", "age" ).eq( 20 ) )
                .add( AuditEntity.property( "a", "streetNumber" ).eq( 1234 )
            )
        )
        .addOrder( AuditEntity.property( "make" ).asc() );`</pre>
    </div>
    </div>
    <div class="paragraph">

    Lastly, this example illustrates how related entity properties can be compared as a constraint.  This query shows how to
    find the `Car` entities where the owner&#8217;s `age` equals the `streetNumber` of where the owner lives:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`AuditQuery query = getAuditReader().createQuery()
        .forEntitiesAtRevision( Car.class, 1 )
        .traverseRelation( "owner", JoinType.INNER, "p" )
        .traverseRelation( "address", JoinType.INNER, "a" )
        .up()
        .up()
        .add( AuditEntity.property( "p", "age" ).eqProperty( "a", "streetNumber" ) );`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">