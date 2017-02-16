 ### 15.52. Group by

    <div class="paragraph">

    The `GROUP BY` clause allows building aggregated results for various value groups. As an example, consider the following queries:

    </div>
    <div id="hql-group-by-example" class="exampleblock">
    <div class="title">Example 404. Group by example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`Long totalDuration = entityManager.createQuery(
        "select sum( c.duration ) " +
        "from Call c ", Long.class )
    .getSingleResult();

    List&lt;Object[]&gt; personTotalCallDurations = entityManager.createQuery(
        "select p.name, sum( c.duration ) " +
        "from Call c " +
        "join c.phone ph " +
        "join ph.person p " +
        "group by p.name", Object[].class )
    .getResultList();

    //It's even possible to group by entities!
    List&lt;Object[]&gt; personTotalCallDurations = entityManager.createQuery(
        "select p, sum( c.duration ) " +
        "from Call c " +
        "join c.phone ph " +
        "join ph.person p " +
        "group by p", Object[].class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The first query retrieves the complete total of all orders.
    The second retrieves the total for each customer, grouped by each customer.

    </div>
    <div class="paragraph">

    In a grouped query, the where clause applies to the non-aggregated values (essentially it determines whether rows will make it into the aggregation).
    The `HAVING` clause also restricts results, but it operates on the aggregated values.
    In the [Group by example](#hql-group-by-example), we retrieved `Call` duration totals for all persons.
    If that ended up being too much data to deal with, we might want to restrict the results to focus only on customers with a summed total of more than 1000:

    </div>
    <div id="hql-group-by-having-example" class="exampleblock">
    <div class="title">Example 405. Having example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Object[]&gt; personTotalCallDurations = entityManager.createQuery(
        "select p.name, sum( c.duration ) " +
        "from Call c " +
        "join c.phone ph " +
        "join ph.person p " +
        "group by p.name " +
        "having sum( c.duration ) &gt; 1000", Object[].class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    <div class="paragraph">

    The `HAVING` clause follows the same rules as the `WHERE` clause and is also made up of predicates.
    `HAVING` is applied after the groupings and aggregations have been done, while the `WHERE` clause is applied before.

    </div>
    </div>
    <div class="sect2">