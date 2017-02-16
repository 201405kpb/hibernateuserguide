 ### 29.10. Projections, aggregation and grouping

    <div class="paragraph">

    The class `org.hibernate.criterion.Projections` is a factory for `Projection` instances.
    You can apply a projection to a query by calling `setProjection()`.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .setProjection( Projections.rowCount() )
        .add( Restrictions.eq("color", Color.BLACK) )
        .list();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .setProjection( Projections.projectionList()
            .add( Projections.rowCount() )
            .add( Projections.avg("weight") )
            .add( Projections.max("weight") )
            .add( Projections.groupProperty("color") )
        )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    There is no explicit "group by" necessary in a criteria query.
    Certain projection types are defined to be _grouping projections_, which also appear in the SQL `group by` clause.

    </div>
    <div class="paragraph">

    An alias can be assigned to a projection so that the projected value can be referred to in restrictions or orderings.
    Here are two different ways to do this:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .setProjection( Projections.alias( Projections.groupProperty("color"), "colr" ) )
        .addOrder( Order.asc("colr") )
        .list();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .setProjection( Projections.groupProperty("color").as("colr") )
        .addOrder( Order.asc("colr") )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    The `alias()` and `as()` methods simply wrap a projection instance in another, aliased, instance of `Projection`.
    As a shortcut, you can assign an alias when you add the projection to a projection list:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .setProjection( Projections.projectionList()
            .add( Projections.rowCount(), "catCountByColor" )
            .add( Projections.avg("weight"), "avgWeight" )
            .add( Projections.max("weight"), "maxWeight" )
            .add( Projections.groupProperty("color"), "color" )
        )
        .addOrder( Order.desc("catCountByColor") )
        .addOrder( Order.desc("avgWeight") )
        .list();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Domestic.class, "cat")
        .createAlias("kittens", "kit")
        .setProjection( Projections.projectionList()
            .add( Projections.property("cat.name"), "catName" )
            .add( Projections.property("kit.name"), "kitName" )
        )
        .addOrder( Order.asc("catName") )
        .addOrder( Order.asc("kitName") )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    You can also use `Property.forName()` to express projections:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .setProjection( Property.forName("name") )
        .add( Property.forName("color").eq(Color.BLACK) )
        .list();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .setProjection( Projections.projectionList()
            .add( Projections.rowCount().as("catCountByColor") )
            .add( Property.forName("weight").avg().as("avgWeight") )
            .add( Property.forName("weight").max().as("maxWeight") )
            .add( Property.forName("color").group().as("color" )
        )
        .addOrder( Order.desc("catCountByColor") )
        .addOrder( Order.desc("avgWeight") )
        .list();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">