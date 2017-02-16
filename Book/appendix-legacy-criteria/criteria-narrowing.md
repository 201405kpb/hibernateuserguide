 ### 29.3. Narrowing the result set

    <div class="paragraph">

    An individual query criterion is an instance of the interface `org.hibernate.criterion.Criterion`.
    The class `org.hibernate.criterion.Restrictions` defines factory methods for obtaining certain built-in `Criterion` types.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.like("name", "Fritz%") )
        .add( Restrictions.between("weight", minWeight, maxWeight) )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    Restrictions can be grouped logically.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.like("name", "Fritz%") )
        .add( Restrictions.or(
            Restrictions.eq( "age", new Integer(0) ),
            Restrictions.isNull("age")
        ) )
        .list();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.in( "name", new String[] { "Fritz", "Izi", "Pk" } ) )
        .add( Restrictions.disjunction()
            .add( Restrictions.isNull("age") )
            .add( Restrictions.eq("age", new Integer(0) ) )
            .add( Restrictions.eq("age", new Integer(1) ) )
            .add( Restrictions.eq("age", new Integer(2) ) )
        ) )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    There are a range of built-in criterion types (`Restrictions` subclasses).
    One of the most useful `Restrictions` allows you to specify SQL directly.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.sqlRestriction("lower({alias}.name) like lower(?)", "Fritz%", Hibernate.STRING) )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    The `{alias}` placeholder will be replaced by the row alias of the queried entity.

    </div>
    <div class="paragraph">

    You can also obtain a criterion from a `Property` instance.
    You can create a `Property` by calling `Property.forName()`:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Property age = Property.forName("age");
    List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.disjunction()
            .add( age.isNull() )
            .add( age.eq( new Integer(0) ) )
            .add( age.eq( new Integer(1) ) )
            .add( age.eq( new Integer(2) ) )
        ) )
        .add( Property.forName("name").in( new String[] { "Fritz", "Izi", "Pk" } ) )
        .list();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">
