 ### 15.53. Order by

    <div class="paragraph">

    The results of the query can also be ordered.
    The `ORDER BY` clause is used to specify the selected values to be used to order the result.
    The types of expressions considered valid as part of the `ORDER BY` clause include:

    </div>
    <div class="ulist">

*   state fields
*   component/embeddable attributes
*   scalar expressions such as arithmetic operations, functions, etc.
*   identification variable declared in the select clause for any of the previous expression types
    </div>
    <div class="paragraph">

    Additionally, JPQL says that all values referenced in the `ORDER BY` clause must be named in the `SELECT` clause.
    HQL does not mandate that restriction, but applications desiring database portability should be aware that not all databases support referencing values in the `ORDER BY` clause that are not referenced in the select clause.

    </div>
    <div class="paragraph">

    Individual expressions in the order-by can be qualified with either `ASC` (ascending) or `DESC` (descending) to indicated the desired ordering direction.
    Null values can be placed in front or at the end of the sorted set using `NULLS FIRST` or `NULLS LAST` clause respectively.

    </div>
    <div id="hql-order-by-example" class="exampleblock">
    <div class="title">Example 406. Order by example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "order by p.name", Person.class )
    .getResultList();

    List&lt;Object[]&gt; personTotalCallDurations = entityManager.createQuery(
        "select p.name, sum( c.duration ) as total " +
        "from Call c " +
        "join c.phone ph " +
        "join ph.person p " +
        "group by p.name " +
        "order by total", Object[].class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">