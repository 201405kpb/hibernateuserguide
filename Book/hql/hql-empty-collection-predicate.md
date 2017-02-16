 ### 15.46. Empty collection predicate

    <div class="paragraph">

    The `IS [NOT] EMPTY` expression applies to collection-valued path expressions.
    It checks whether the particular collection has any associated values.

    </div>
    <div id="hql-empty-collection-predicate-example" class="exampleblock">
    <div class="title">Example 402. Empty collection expression examples</div>
    <div class="content">
    <div class="listingblock">
    <div class="content">
    <pre class="prettyprint highlight">`List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.phones is empty", Person.class )
    .getResultList();

    List&lt;Person&gt; persons = entityManager.createQuery(
        "select p " +
        "from Person p " +
        "where p.phones is not empty", Person.class )
    .getResultList();`</pre>
    </div>
    </div>
    </div>
    </div>
    </div>
    <div class="sect2">