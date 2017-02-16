 ### 29.11. Detached queries and subqueries

    <div class="paragraph">

    The `DetachedCriteria` class allows you to create a query outside the scope of a session and then execute it using an arbitrary `Session`.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DetachedCriteria query = DetachedCriteria.forClass(Cat.class)
        .add( Property.forName("sex").eq('F') );

    Session session = ....;
    Transaction txn = session.beginTransaction();
    List results = query.getExecutableCriteria(session).setMaxResults(100).list();
    txn.commit();
    session.close();`</pre>
    </div>
    </div>
    <div class="paragraph">

    A `DetachedCriteria` can also be used to express a subquery.
    `Criterion` instances involving subqueries can be obtained via `Subqueries` or `Property`.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DetachedCriteria avgWeight = DetachedCriteria.forClass(Cat.class)
        .setProjection( Property.forName("weight").avg() );
    session.createCriteria(Cat.class)
        .add( Property.forName("weight").gt(avgWeight) )
        .list();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DetachedCriteria weights = DetachedCriteria.forClass(Cat.class)
        .setProjection( Property.forName("weight") );
    session.createCriteria(Cat.class)
        .add( Subqueries.geAll("weight", weights) )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    Correlated subqueries are also possible:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DetachedCriteria avgWeightForSex = DetachedCriteria.forClass(Cat.class, "cat2")
        .setProjection( Property.forName("weight").avg() )
        .add( Property.forName("cat2.sex").eqProperty("cat.sex") );
    session.createCriteria(Cat.class, "cat")
        .add( Property.forName("weight").gt(avgWeightForSex) )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    Example of multi-column restriction based on a subquery:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`DetachedCriteria sizeQuery = DetachedCriteria.forClass( Man.class )
        .setProjection( Projections.projectionList().add( Projections.property( "weight" ) )
                                                    .add( Projections.property( "height" ) ) )
        .add( Restrictions.eq( "name", "John" ) );
    session.createCriteria( Woman.class )
        .add( Subqueries.propertiesEq( new String[] { "weight", "height" }, sizeQuery ) )
        .list();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">