### 29.6. Dynamic association fetching

    <div class="paragraph">

    You can specify association fetching semantics at runtime using `setFetchMode()`.

    </div>
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List cats = sess.createCriteria(Cat.class)
        .add( Restrictions.like("name", "Fritz%") )
        .setFetchMode("mate", FetchMode.EAGER)
        .setFetchMode("kittens", FetchMode.EAGER)
        .list();`</pre>
    </div>
    </div>
    <div class="paragraph">

    This query will fetch both `mate` and `kittens` by outer join.

    </div>
    </div>
    <div class="sect2">