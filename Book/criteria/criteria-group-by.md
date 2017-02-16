### 16.13. Using group by

    <div id="criteria-group-by-example" class="exampleblock">
    <div class="title">Example 421. Group by example</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`CriteriaBuilder builder = entityManager.getCriteriaBuilder();

    CriteriaQuery&lt;Tuple&gt; criteria = builder.createQuery( Tuple.class );
    Root&lt;Person&gt; root = criteria.from( Person.class );

    criteria.groupBy(root.get("address"));
    criteria.multiselect(root.get("address"), builder.count(root));

    List&lt;Tuple&gt; tuples = entityManager.createQuery( criteria ).getResultList();

    for ( Tuple tuple : tuples ) {
        String name = (String) tuple.get( 0 );
        Long count = (Long) tuple.get( 1 );
    }`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect1">