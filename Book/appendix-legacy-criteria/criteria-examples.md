 ### 29.9. Example queries

    <div class="paragraph">

    The class `org.hibernate.criterion.Example` allows you to construct a query criterion from a given instance.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Cat cat = new Cat();
    cat.setSex('F');
    cat.setColor(Color.BLACK);
    List results = session.createCriteria(Cat.class)
        .add( Example.create(cat) )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    Version properties, identifiers and associations are ignored.
    By default, null valued properties are excluded.

    </div>
    <div class="paragraph">

    You can adjust how the `Example` is applied.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Example example = Example.create(cat)
        .excludeZeroes()           //exclude zero valued properties
        .excludeProperty("color")  //exclude the property named "color"
        .ignoreCase()              //perform case insensitive string comparisons
        .enableLike();             //use like for string comparisons
    List results = session.createCriteria(Cat.class)
        .add(example)
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    You can even use examples to place criteria upon associated objects.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List results = session.createCriteria(Cat.class)
        .add( Example.create(cat) )
        .createCriteria("mate")
            .add( Example.create( cat.getMate() ) )
        .list();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">