 ### 29.4. Ordering the results

    <div class="paragraph">

    You can order the results using `org.hibernate.criterion.Order`.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.like("name", "F%")
        .addOrder( Order.asc("name").nulls(NullPrecedence.LAST) )
        .addOrder( Order.desc("age") )
        .setMaxResults(50)
        .list();`</pre>
    </div>
    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Property.forName("name").like("F%") )
        .addOrder( Property.forName("name").asc() )
        .addOrder( Property.forName("age").desc() )
        .setMaxResults(50)
        .list();`</pre>
    </div>
    </div>
    </div>
    <div class="sect2">