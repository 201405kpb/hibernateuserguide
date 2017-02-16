 ### 29.5. Associations

    <div class="paragraph">

    By navigating associations using `createCriteria()` you can specify constraints upon related entities:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.like("name", "F%") )
        .createCriteria("kittens")
            .add( Restrictions.like("name", "F%") )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    The second `createCriteria()` returns a new instance of `Criteria` that refers to the elements of the `kittens` collection.

    </div>
    <div class="paragraph">

    There is also an alternate form that is useful in certain circumstances:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .createAlias("kittens", "kt")
        .createAlias("mate", "mt")
        .add( Restrictions.eqProperty("kt.name", "mt.name") )
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    (`createAlias()` does not create a new instance of `Criteria`.)

    </div>
    <div class="paragraph">

    The kittens collections held by the `Cat` instances returned by the previous two queries are _not_ pre-filtered by the criteria.
    If you want to retrieve just the kittens that match the criteria, you must use a `ResultTransformer`.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .createCriteria("kittens", "kt")
            .add( Restrictions.eq("name", "F%") )
        .setResultTransformer(Criteria.ALIAS_TO_ENTITY_MAP)
        .list();
    Iterator iter = cats.iterator();
    while ( iter.hasNext() ) {
        Map map = (Map) iter.next();
        Cat cat = (Cat) map.get(Criteria.ROOT_ALIAS);
        Cat kitten = (Cat) map.get("kt");
    }`</pre>
    </div>
    </div>
    <div class="paragraph">

    Additionally, you may manipulate the result set using a left outer join:

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = session.createCriteria( Cat.class )
       .createAlias("mate", "mt", Criteria.LEFT_JOIN, Restrictions.like("mt.name", "good%") )
       .addOrder(Order.asc("mt.age"))
       .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    This will return all of the `Cat`s with a mate whose name starts with "good" ordered by their mate&#8217;s age, and all cats who do not have a mate.
    This is useful when there is a need to order or limit in the database prior to returning complex/large result sets,
    and removes many instances where multiple queries would have to be performed and the results unioned by java in memory.

    </div>
    <div class="paragraph">

    Without this feature, first all of the cats without a mate would need to be loaded in one query.

    </div>
    <div class="paragraph">

    A second query would need to retrieve the cats with mates who&#8217;s name started with "good" sorted by the mates age.

    </div>
    <div class="paragraph">

    Thirdly, in memory; the lists would need to be joined manually.

    </div>
    </div>
    <div class="sect2">